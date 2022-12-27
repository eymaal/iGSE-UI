<script>
  import Router from 'svelte-spa-router';
  import Login from './pages/Login-Signup/Login.svelte'
  import Dashboard from './pages/Dashboard.svelte';
  import Registration from './pages/Login-Signup/Registration.svelte';

  const routes = {
    '/': Login,
    '/login': Login,
    '/registration': Registration,
    '/dashboard': Dashboard,
    '*': Login,
  }

</script>

<Router {routes}/>
