<script>
  import Navbar from '../Navbar.svelte';
  import { push } from 'svelte-spa-router';
  import { onMount } from 'svelte';
  import Footer from './util/Footer.svelte';

  let readings = [];
  let customer_id;
  if(localStorage.getItem('customer')){
    customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
  }

  async function fetchReading() {
    const res = await fetch('http://localhost:8080/iGSE/getReadings',{
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({customer_id : customer_id})
    })
    .then((response) => {
      
      if(!response.ok){
        readings = [];
        return response.json();          
      } else{
        return response.json();
    }})
    .then((result) => {
      readings = result;
    })
  }
  onMount(async () => {
    if(!localStorage.getItem('customer')){
      push('/login');
    } else{
      if(JSON.parse(localStorage.getItem('customer')).type=='admin'){
        push('/admin');
      }
    }
    fetchReading();    
  })

  let actions = [
    {
      title: "Add Reading",
      description: "Click here to add new meter readings.",
      path: "/addReading"
    },
    {
      title: "Topup Balance",
      description: "Add Energy Code Vouchers to account.",
      path: "/recharge"
    },
    {
      title: "Pay Bills",
      description: "Pay your unpaid bills.",
      path: "/billpay"
    }
  ]

</script> 

<Navbar/>

<div id="dash-container" class="w-full h-auto bg-base-200 p-2 grid place-items-center pb-4">
  {#each actions as action}
    <div class="card card-compact w-80 bg-base-100 shadow-xl cursor-pointer">
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <div class="card-body" on:click={()=> push(action.path)}>
        <h2 class="card-title">{action.title}</h2>
        <p>{action.description}</p>
      </div>
    </div>
  {/each}

  {#if readings.length>0}
    <div class="overflow-x-auto span-all-columns w-full flex justify-center">
      <div class="table-container">        
        <table class="table table-compact w-full">
          <thead>
            <tr>
              <th></th>
              <th>Submission</th>
              <th>⚡Day Units</th>
              <th>⚡Night Units</th>
              <th>Gas Units</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            {#each readings as reading, index }  
              <tr class="hover">
                <th>{index+1}</th>
                <td>{reading.submission_date.slice(0,10)}</td>
                <td>{reading.elec_readings_day}</td>
                <td>{reading.elec_readings_night}</td>
                <td>{reading.gas_reading}</td>
                <td>{reading.status}</td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  {:else}
  <p class = "text-primary">
    <span>
      No readings yet. Submit your first reading to view history.
    </span>
  </p>
  {/if}
</div>

<Footer/>

<style>
  #dash-container{
    gap: 12px;
    grid-template-columns: repeat(auto-fit, minmax(330px, 1fr));
    min-height: calc(100vh - 278px - 64px);
  }
  .card:hover{
    color: hsl(var(--p)) !important;
  }
  .span-all-columns{
    grid-column: 1/-1;
  }
  .span-all-columns > table{
    width: calc(0.95 * 100vh);
  }
  .table thead th{
    @apply bg-primary;
    color: hsl(var(--b1));
  }
</style>