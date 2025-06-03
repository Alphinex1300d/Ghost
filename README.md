# Algoritmo de Recomendação Musical com Spotify e PySpark

Este projeto resolve o problema: **"Não sei qual meu gosto musical preferido."**  
Criamos um algoritmo com dados do Spotify para identificar músicas com as quais o usuário mais se identifica.

## 🧠 Problema
Muitas pessoas não conseguem definir seus gostos musicais. Isso prejudica o uso de plataformas como o Spotify que dependem de preferências claras.

## 💡 Solução
Usamos a API do Spotify para coletar as músicas mais ouvidas de um usuário e tratamos os dados com PySpark. Com isso, conseguimos visualizar padrões de popularidade e preferências.

## 🔧 Tecnologias
- Python + Google Colab
- PySpark
- Spotify Web API
- Matplotlib

## 📊 Resultado
Visualização de músicas mais ouvidas com base nas reproduções reais, revelando o estilo musical do usuário com clareza.

## 🔐 Autenticação
1. Vá até [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) e crie um app.
2. Coloque este redirect URI: `https://example.com/callback`
3. Gere seu token manualmente neste site: https://developer.spotify.com/console/get-current-user-top-artists-and-tracks/
4. Copie o token e cole no notebook quando solicitado.

## 📎 Como rodar
Abra o notebook `spotify_recommender.ipynb` no [Google Colab](https://colab.research.google.com/) e siga os passos. Ele irá:
- Autenticar sua conta
- Coletar suas músicas mais ouvidas
- Visualizar as mais populares
