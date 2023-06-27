<script lang="ts">
    import ResultCard from "./lib/ResultCard.svelte";

  const CLIENT_ID = '003e2875a63b4ae280acf06f5a70c549'
  const PARAMS = new URLSearchParams(window.location.search)
  const code = PARAMS.get("code")
  let showCard = false
  let swiftieResult = false

  const redirectToAuthCodeFlow = async (clientId: string) => {
    const verifier = generateCodeVerifier(128)
    const challenge = await generateCodeChallenge(verifier)

    localStorage.setItem("verifier", verifier)

    const params = new URLSearchParams()
    params.append("client_id", clientId)
    params.append("response_type", "code")
    params.append("redirect_uri", "http://localhost:5173")
    params.append("scope", "user-read-private user-read-email user-top-read")
    params.append("code_challenge_method", "S256")
    params.append("code_challenge", challenge)
    
    document.location = `https://accounts.spotify.com/authorize?${params.toString()}`
  }

  const generateCodeVerifier = (length: number) => {
    let text = ''
    let possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'

    for (let i = 0; i < length; i++) {
      text += possible.charAt(Math.floor(Math.random() * possible.length))
    }
    return text
  }

  const generateCodeChallenge = async (codeVerifier: string) => {
    const data = new TextEncoder().encode(codeVerifier)
    const digest = await window.crypto.subtle.digest('SHA-256', data)
    console.log(digest);

    return btoa(String.fromCharCode.apply(null, [...new Uint8Array(digest)]))
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=+$/, '');
  }

  const getAccessToken = async (clientId: string, code: string) => {
    const verifier = localStorage.getItem("verifier")

    const params = new URLSearchParams();
    params.append("client_id", clientId);
    params.append("grant_type", "authorization_code");
    params.append("code", code);
    params.append("redirect_uri", "http://localhost:5173");
    params.append("code_verifier", verifier!);

    const result = await fetch("https://accounts.spotify.com/api/token", {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: params
    });

    const { access_token } = await result.json();
    return access_token;
  }

  const fetchTracks = async (token) => {
    const result = await fetch("https://api.spotify.com/v1/me/top/tracks?time_range=long_term&limit=50&offset=0", {
      method: "GET", headers: { Authorization: `Bearer ${token}` }
    });

    return await result.json();
  }

  const calculateSwiftiePercentage = (tracksList, total) => {
    let taylorPlays = 0

    tracksList.forEach(track => {
      if (track.artists.some(artist => artist.name === "Taylor Swift")) {
        taylorPlays++
      }
    });

    return taylorPlays
  }

  const renderResult = (result) => {
    if (result < 50) {
      swiftieResult = false
      showCard = true
    } else {
      swiftieResult = true
      showCard = true
    }
  }

  const checkProfile = async () => {
    if (!code) {
      redirectToAuthCodeFlow(CLIENT_ID);
    } else {
      const accessToken = await getAccessToken(CLIENT_ID, code)
      // const profile = await fetchProfile(accessToken)
      const tracks = await fetchTracks(accessToken)
      const result = calculateSwiftiePercentage(tracks.items, tracks.total)
      renderResult(result)
    }
  }

import tsPNG from './assets/taylor.png'
import crPNG from './assets/celso.png'

</script>

<main>
  {#if showCard}
    <ResultCard result={swiftieResult}/>
  {/if}
  <div class="photos-container">
    <img src={crPNG} alt="Celso Russomano"/>
    <img src={tsPNG} alt="Taylor Swift"/>
  </div>
  <h1>Você é CAMBISTA ou SWIFTIE?</h1>
  <section class="content-container">
    <span>Entre com seu Spotify e descubra se você seria preso pelo Celso Russomano ou vai poder ver o show da Loirinha</span>

    <button on:click={checkProfile} class="spotify-button">Entrar com Spotify</button>
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
    height: 300px;
    object-fit: cover;
  }
</style>
