<script>
  import Router from 'svelte-spa-router';
  import Login from './pages/Login-Signup/Login.svelte'
  import Dashboard from './pages/Dashboard/Dashboard.svelte';
  import Registration from './pages/Login-Signup/Registration.svelte';
  import NewReading from './pages/Dashboard/NewReading.svelte';
  import Recharge from './pages/Dashboard/Recharge.svelte';

  const routes = {
    '/': Login,
    '/login': Login,
    '/registration': Registration,
    '/dashboard': Dashboard,
    '/addReading': NewReading,
    '/recharge': Recharge,

    '*': Login,
  }

  let messages={
    content: "",
    type: "",
    action: "",
    path: ""
  }

</script>

{#if messages.content!=""}
  <div id="notification" class="fixed z-50 w-full">
    <div class="alert shadow-lg mx-auto my-4">
      <div class="self-start">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="stroke-info flex-shrink-0 w-6 h-6"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
        <span>{messages.content}</span>
      </div>
    </div>
  </div>
{/if}


<Router {routes}/>

<style>
  #notification > .alert{
    width: calc(100vw - 32px);
  }
</style>