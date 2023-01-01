<script>
  import Router from 'svelte-spa-router';
  import Login from './pages/Login-Signup/Login.svelte'
  import Dashboard from './pages/Dashboard/Dashboard.svelte';
  import Registration from './pages/Login-Signup/Registration.svelte';
  import NewReading from './pages/Dashboard/NewReading.svelte';
  import Recharge from './pages/Dashboard/Recharge.svelte';
  import Messages from './util/Messages.svelte';
  import PayBills from './pages/Dashboard/PayBills.svelte';
  import ErrorPage from './pages/404.svelte';
  import MessageStore from './MessageStore';
  import Admin from './pages/Dashboard/util/Admin.svelte';

  const routes = {
    '/': Login,
    '/login': Login,
    '/registration': Registration,
    '/dashboard': Dashboard,
    '/addReading': NewReading,
    '/recharge': Recharge,
    '/billpay' : PayBills,
    '/admin' : Admin,
    '*': ErrorPage,
  }

  export let message ={
    content: "",
    action: "",
    path: "",
    type: ""
  };
  
  MessageStore.subscribe(data => {
    message = data;
  });


</script>

<Messages {...message}/>
<Router {routes}/>
