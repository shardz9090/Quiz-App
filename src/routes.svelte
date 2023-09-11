<script>
import Route from "svelte-spa-router"
import Router from "svelte-spa-router";
import Home from './App.svelte';
import Quest from './Quest.svelte';

</script>



<Router>
  <Route path="/" component={Home} />
  <Route path="/quest" component={Quest} />
</Router>

