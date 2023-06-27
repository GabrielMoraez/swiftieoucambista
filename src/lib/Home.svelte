<script lang="ts">
  import tsPNG from '../assets/taylor.png'
  import crPNG from '../assets/celso.png'

  const clientId = import.meta.env.VITE_CLIENT_ID
  const redirectUri = import.meta.env.VITE_REDIRECT_URI

  const scopes = ['user-top-read', 'user-read-private']

  const authorizationUrl = `https://accounts.spotify.com/authorize?client_id=${clientId}&response_type=token&redirect_uri=${encodeURIComponent(redirectUri)}&scope=${encodeURIComponent(scopes.join(' '))}`;

  const redirectToAuthCodeFlow = async () => {
    window.location.href = authorizationUrl
  }

</script>

<main>
<div class="photos-container">
  <img src={crPNG} alt="Celso Russomano"/>
  <img src={tsPNG} alt="Taylor Swift"/>
</div>
<h1>Você é CAMBISTA ou SWIFTIE?</h1>
<section class="content-container">
  <span>Entre com seu Spotify e descubra se você seria preso pelo Celso Russomano ou vai poder ver o show da Loirinha</span>

  <button on:click={redirectToAuthCodeFlow} class="spotify-button">Entrar com Spotify</button>
</section>
</main>

<style>
.photos-container {
  display: flex;
  justify-content: space-between;
}
.content-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 25px;
}
.spotify-button {
  width: 200px;
  background-color: #1DB954;
  color: #FFF;
  border: none;
  transition: ease-in-out 0.2s;
}
.spotify-button:hover {
  cursor: pointer;
  background-color: #191414;
  overflow: none;
  border: none;
  transition: ease-in-out 0.2s;
}
img {
  height: 250px;
  object-fit: cover;
}
</style>
