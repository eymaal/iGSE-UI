<script>
  import Router from 'svelte-spa-router';
  import Login from './pages/Login-Signup/Login.svelte'
  import Dashboard from './pages/Dashboard/Dashboard.svelte';
  import Registration from './pages/Login-Signup/Registration.svelte';
  import NewReading from './pages/Dashboard/NewReading.svelte';

  const routes = {
    '/': Login,
    '/login': Login,
    '/registration': Registration,
    '/dashboard': Dashboard,
    '/addReading': NewReading,

    '*': Login,
  }

</script>

<Router {routes}/>
