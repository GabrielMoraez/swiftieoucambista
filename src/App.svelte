<script>
  import { Router, Route } from 'svelte-routing'
  import Home from './lib/Home.svelte'
  import Callback from './lib/Callback.svelte'
  import Result from './lib/Result.svelte'
</script>

<Router>
  <Route path="/" component={Home} />
  <Route path="/callback" component={Callback} />
  <Route path="/result" component={Result} />
</Router>