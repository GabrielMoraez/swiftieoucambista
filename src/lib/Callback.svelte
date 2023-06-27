<script>
  import { onMount } from 'svelte';
  import { navigate } from 'svelte-routing';

  onMount(() => {
    const searchParams = new URLSearchParams(window.location.hash.substr(1));
    const accessToken = searchParams.get('access_token');

    localStorage.setItem('access_token', accessToken);
    navigate('/result')
  })
  
</script>

<p>Authenticating...</p>
