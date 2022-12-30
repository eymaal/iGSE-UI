<script>
  
  import { onMount } from 'svelte'
    import { push } from 'svelte-spa-router';
  import { themeChange } from 'theme-change'

  // NOTE: the element that is using one of the theme attributes must be in the DOM on mount
  onMount(() => {
    themeChange(false);
  })
  
  let theme;
  let logout = () => {
    if(localStorage.getItem('customer')){
      localStorage.setItem('lastLogin', JSON.parse(localStorage.getItem('customer')).customer_id);
      localStorage.removeItem('customer');
    }
    push('/login');    
  }

</script>
<div class="navbar bg-base-100">
    <div class="navbar-start">
      <div class="dropdown">
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <label for="hamburger" tabindex="0" class="btn btn-ghost btn-circle">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h7" /></svg>
        </label>
        
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <ul tabindex="0" class="menu menu-compact lg:menu-normal dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52">
          <li><a href="/#/addReading">Add Reading</a></li>
          <li><a href="/#/recharge">Add E-Voucher</a></li>
          <li><a href="/#/Dashboard">Pay Bills</a></li>
          <li><a href="/#/login">Log Out</a></li>
        </ul>
      </div>
    </div>
    <div class="navbar-center">
      <a href = "/#/Dashboard" class="btn btn-ghost normal-case text-xl">iGSE</a>
    </div>
    <div class="navbar-end">
      <button id="theme" data-toggle-theme="fantasy,dracula" data-act-class="ACTIVECLASS" bind:this={theme} class="btn btn-ghost btn-circle">
        <label class="swap swap-rotate w-full h-full">
          <!-- this hidden checkbox controls the state -->
          <input class = "w-full h-full" type="checkbox" on:change={() => {theme.click();} }/>        
          <!-- sun icon -->
          <svg class="swap-off fill-current w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M5.64,17l-.71.71a1,1,0,0,0,0,1.41,1,1,0,0,0,1.41,0l.71-.71A1,1,0,0,0,5.64,17ZM5,12a1,1,0,0,0-1-1H3a1,1,0,0,0,0,2H4A1,1,0,0,0,5,12Zm7-7a1,1,0,0,0,1-1V3a1,1,0,0,0-2,0V4A1,1,0,0,0,12,5ZM5.64,7.05a1,1,0,0,0,.7.29,1,1,0,0,0,.71-.29,1,1,0,0,0,0-1.41l-.71-.71A1,1,0,0,0,4.93,6.34Zm12,.29a1,1,0,0,0,.7-.29l.71-.71a1,1,0,1,0-1.41-1.41L17,5.64a1,1,0,0,0,0,1.41A1,1,0,0,0,17.66,7.34ZM21,11H20a1,1,0,0,0,0,2h1a1,1,0,0,0,0-2Zm-9,8a1,1,0,0,0-1,1v1a1,1,0,0,0,2,0V20A1,1,0,0,0,12,19ZM18.36,17A1,1,0,0,0,17,18.36l.71.71a1,1,0,0,0,1.41,0,1,1,0,0,0,0-1.41ZM12,6.5A5.5,5.5,0,1,0,17.5,12,5.51,5.51,0,0,0,12,6.5Zm0,9A3.5,3.5,0,1,1,15.5,12,3.5,3.5,0,0,1,12,15.5Z"/></svg>          
          <!-- moon icon -->
          <svg class="swap-on fill-current w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M21.64,13a1,1,0,0,0-1.05-.14,8.05,8.05,0,0,1-3.37.73A8.15,8.15,0,0,1,9.08,5.49a8.59,8.59,0,0,1,.25-2A1,1,0,0,0,8,2.36,10.14,10.14,0,1,0,22,14.05,1,1,0,0,0,21.64,13Zm-9.5,6.69A8.14,8.14,0,0,1,7.08,5.22v.27A10.15,10.15,0,0,0,17.22,15.63a9.79,9.79,0,0,0,2.1-.22A8.11,8.11,0,0,1,12.14,19.73Z"/></svg>         
        </label>
      </button>

      <div class="dropdown dropdown-end">
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <label for="profile" tabindex="0" class="btn btn-ghost btn-circle">
          <svg stroke="currentColor" fill="currentColor" class = "h-5 w-5" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"> <title></title> <g id="about"> <path d="M16,16A7,7,0,1,0,9,9,7,7,0,0,0,16,16ZM16,4a5,5,0,1,1-5,5A5,5,0,0,1,16,4Z"></path> <path d="M17,18H15A11,11,0,0,0,4,29a1,1,0,0,0,1,1H27a1,1,0,0,0,1-1A11,11,0,0,0,17,18ZM6.06,28A9,9,0,0,1,15,20h2a9,9,0,0,1,8.94,8Z"></path> </g> </g></svg>
        </label>
        
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <ul tabindex="0" class="lg:menu-normal menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52">
          <!-- svelte-ignore a11y-invalid-attribute -->
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <!-- svelte-ignore a11y-missing-attribute -->
          <li><a on:click={logout}>Log out</a></li>
        </ul>
      </div>
      
    </div>
  </div>