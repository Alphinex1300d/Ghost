rom pyspark.sql import SparkSession
from pyspark.sql.functions import to_timestamp, hour, dayofmonth, avg
from pyspark.sql.types import StructType, StructField, StringType, FloatType
import requests

# Inicializa a SparkSession
spark = SparkSession.builder \
    .appName("MeteorologicalDataAnalysis") \
    .getOrCreate()

# URL da API Open-Meteo (exemplo para Berlim)
# Você pode ajustar a latitude e longitude conforme necessário
api_url = "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m"

# Faz a requisição à API
response = requests.get(api_url)
data = response.json()

# Define o schema para o DataFrame Spark
schema = StructType([
    StructField("time", StringType(), True),
    StructField("temperature_2m", FloatType(), True),
    StructField("relative_humidity_2m", FloatType(), True),
    StructField("wind_speed_10m", FloatType(), True)
])

# Extrai os dados horários
hourly_data = data["hourly"]

# Prepara os dados para o DataFrame Spark
processed_data = []
for i in range(len(hourly_data["time"])):
    processed_data.append({
        "time": hourly_data["time"][i],
        "temperature_2m": float(hourly_data["temperature_2m"][i]),
        "relative_humidity_2m": float(hourly_data["relative_humidity_2m"][i]),
        "wind_speed_10m": float(hourly_data["wind_speed_10m"][i])
    })

# Cria o DataFrame Spark
df = spark.createDataFrame(processed_data, schema=schema)

# Análise Temporal
# Converte a coluna 'time' para tipo timestamp
df_time = df.withColumn("timestamp", to_timestamp("time"))

# Extrai a hora e o dia do mês
df_time = df_time.withColumn("hour", hour("timestamp")) \
                   .withColumn("day", dayofmonth("timestamp"))

# Calcula a temperatura média diária
df_daily_avg = df_time.groupBy("day").agg(
    avg("temperature_2m").alias("avg_temperature_2m"),
    avg("relative_humidity_2m").alias("avg_relative_humidity_2m"),
    avg("wind_speed_10m").alias("avg_wind_speed_10m")
).orderBy("day")

print("\nDados Horários:")
df.show()

print("\nDados com Hora e Dia:")
df_time.show()

print("\nTemperaturas Médias Diárias:")
df_daily_avg.show()

# Salva o DataFrame em formato Parquet (pode ser alterado para outro formato)
df.write.mode("overwrite").parquet("/home/ubuntu/weather_data.parquet")

# Para parar a SparkSession (opcional, mas boa prática)
spark.stop()
