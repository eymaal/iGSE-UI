<script>
  import Navbar from '../Navbar.svelte';
  import { push } from 'svelte-spa-router';
  import { onMount } from 'svelte';

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
      path: "/dashboard"
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
  {:else}
  <p class = "text-primary">
    <span>
      No readings yet. Submit your first reading to view history.
    </span>
  </p>
  {/if}
</div>

<footer class="footer footer-center p-10 bg-primary text-primary-content">
  <div>
    <svg width="100" height="100" viewBox="0 0 14 14" role="img" focusable="false" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="#000000" stroke="#000000" stroke-width="0.11200000000000002"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"><path fill="#cddc39" d="M11.00000006 4.99999997C11.00000006 2.77142851 9.22857146.99999991 7 .99999991c-2.22857146 0-4.00000006 1.7714286-4.00000006 4.00000006 0 2.34285718 2.28571432 3.08571433 2.28571432 4.74285721v.40000001h3.42857148v-.4c0-1.65714289 2.28571432-2.40000004 2.28571432-4.74285722z"></path><path fill="#388e3c" d="M5.65714284 2.68571422s-.97142859 2.20000003-.54285715 3.65714291C5.4285714 7.37142858 6.5142857 7.62857143 7 7.25714286c.05714286.42857144-.11428572.80000001-.25714286 1.17142859L7.6 8.74285717c.54285715-1.60000002-.37142858-3.42857148-.91428573-4.28571435 1.2857143.91428573 1.65714288 2.4571429 1.65714288 2.4571429.11428572-.05714286.17142858-.17142858.31428572-.31428573.80000001-.97142858.37142858-3.22857147-3.00000004-3.91428577zM5.28571426 9.8571429v1.42857145c0 .54285715.4 1.00000001.91428573 1.11428573.11428571.34285715.42857143.60000001.80000001.60000001s.71428572-.25714286.80000001-.6c.51428572-.11428573.91428573-.57142859.91428573-1.11428574V9.8571429H5.28571426z"></path><path fill="#4caf50" d="M8.71428574 11.28571435l-3.31428576.45714286c.08571428.2.25714286.40000001.45714286.51428573l2.68571433-.37142858c.11428571-.17142858.17142857-.37142858.17142857-.60000001zm-3.42857148-.08571429l3.42857148-.48571429v-.57142858l-3.42857148.4857143z"></path></g></svg>    
    <label for="company" class="label text-2xl">The Great Shangri-La Energy Company</label>
    <label for="company_tag" class="label p-0">Energy for the people. Energy by the people.</label>
  </div> 
</footer>

<style>
  #dash-container{
    gap: 12px;
    grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
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