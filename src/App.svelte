<script lang="ts">
  import { Route, Router } from "svelte-routing"
  import Translator from './pages/Translator.svelte'
    import History from "./pages/History.svelte";
</script>

<main>
  <Router url="">

    <Route path="/" component={Translator} />
    <Route path="/history" component={History} />
  </Router>


</main>

<style>
</style>
