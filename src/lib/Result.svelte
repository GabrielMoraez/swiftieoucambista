<script>
  import { onMount } from 'svelte'
  import ResultCard from './ResultCard.svelte';

  let showCard = false;
  let swiftieResult = false;
  let profile = {}
  let favoriteMusic = ''

  const fetchTracks = async (token) => {
    const tracksPromise = await fetch("https://api.spotify.com/v1/me/top/tracks?time_range=long_term&limit=50&offset=0", {
      method: "GET", headers: { Authorization: `Bearer ${token}` }
    });
    const profilePromise = await fetch("https://api.spotify.com/v1/me", {
        method: "GET", headers: { Authorization: `Bearer ${token}` }
    });

    const tracks = await tracksPromise.json();
    profile = await profilePromise.json();

    return {tracks};
  }

  const calculateSwiftiePercentage = (tracksList) => {
    let taylorPlays = 0

    tracksList.forEach(track => {
      if (track.artists.some(artist => artist.name === "Taylor Swift")) {
        if (favoriteMusic === '') {
          favoriteMusic = track.name
        }
        taylorPlays++
      }
    });

    return taylorPlays
  }

  const renderResult = (result) => {
    return result > 50
  }

  const test = async (accessToken) => {
    const { tracks } = await fetchTracks(accessToken)
    const result = calculateSwiftiePercentage(tracks.items)
    swiftieResult = renderResult(result);
    showCard = true
  }

  onMount(() => {
    const access_token = localStorage.getItem('access_token')
    test(access_token)
  })

</script>

<div>
  {#if showCard}
    <ResultCard {favoriteMusic} result={swiftieResult} {profile}/>
  {/if}
</div>