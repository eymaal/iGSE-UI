<script>
    import Navbar from '../Navbar.svelte';
    import icon from '../../assets/oil-drill.svg';
    import {push} from 'svelte-spa-router';
    import {onMount, onDestroy} from 'svelte';

    let readings = [];
    onMount(async () => {
      const res = await fetch('http://localhost:8080/iGSE/getReadings',{
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({customer_id: "we@shangrila.gov.un"})
      })
      .then((response) => {
        
        if(!response.ok){
          //fail case
          readings = [];
          return response.json();          
        } else{
          return response.json();
      }})
      .then((result) => {
        readings = result;
        console.log(readings);
      })

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
        <!-- head -->
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
  {/if}
</div>


<footer class="footer footer-center p-10 bg-primary text-primary-content">
    <div>
      <svg width="100" height="100" viewBox="0 0 14 14" role="img" focusable="false" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="#000000" stroke="#000000" stroke-width="0.11200000000000002"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"><path fill="#cddc39" d="M11.00000006 4.99999997C11.00000006 2.77142851 9.22857146.99999991 7 .99999991c-2.22857146 0-4.00000006 1.7714286-4.00000006 4.00000006 0 2.34285718 2.28571432 3.08571433 2.28571432 4.74285721v.40000001h3.42857148v-.4c0-1.65714289 2.28571432-2.40000004 2.28571432-4.74285722z"></path><path fill="#388e3c" d="M5.65714284 2.68571422s-.97142859 2.20000003-.54285715 3.65714291C5.4285714 7.37142858 6.5142857 7.62857143 7 7.25714286c.05714286.42857144-.11428572.80000001-.25714286 1.17142859L7.6 8.74285717c.54285715-1.60000002-.37142858-3.42857148-.91428573-4.28571435 1.2857143.91428573 1.65714288 2.4571429 1.65714288 2.4571429.11428572-.05714286.17142858-.17142858.31428572-.31428573.80000001-.97142858.37142858-3.22857147-3.00000004-3.91428577zM5.28571426 9.8571429v1.42857145c0 .54285715.4 1.00000001.91428573 1.11428573.11428571.34285715.42857143.60000001.80000001.60000001s.71428572-.25714286.80000001-.6c.51428572-.11428573.91428573-.57142859.91428573-1.11428574V9.8571429H5.28571426z"></path><path fill="#4caf50" d="M8.71428574 11.28571435l-3.31428576.45714286c.08571428.2.25714286.40000001.45714286.51428573l2.68571433-.37142858c.11428571-.17142858.17142857-.37142858.17142857-.60000001zm-3.42857148-.08571429l3.42857148-.48571429v-.57142858l-3.42857148.4857143z"></path></g></svg>
      <!-- <svg width="50" height="50" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill-rule="evenodd" clip-rule="evenodd" class="inline-block fill-current"><path d="M22.672 15.226l-2.432.811.841 2.515c.33 1.019-.209 2.127-1.23 2.456-1.15.325-2.148-.321-2.463-1.226l-.84-2.518-5.013 1.677.84 2.517c.391 1.203-.434 2.542-1.831 2.542-.88 0-1.601-.564-1.86-1.314l-.842-2.516-2.431.809c-1.135.328-2.145-.317-2.463-1.229-.329-1.018.211-2.127 1.231-2.456l2.432-.809-1.621-4.823-2.432.808c-1.355.384-2.558-.59-2.558-1.839 0-.817.509-1.582 1.327-1.846l2.433-.809-.842-2.515c-.33-1.02.211-2.129 1.232-2.458 1.02-.329 2.13.209 2.461 1.229l.842 2.515 5.011-1.677-.839-2.517c-.403-1.238.484-2.553 1.843-2.553.819 0 1.585.509 1.85 1.326l.841 2.517 2.431-.81c1.02-.33 2.131.211 2.461 1.229.332 1.018-.21 2.126-1.23 2.456l-2.433.809 1.622 4.823 2.433-.809c1.242-.401 2.557.484 2.557 1.838 0 .819-.51 1.583-1.328 1.847m-8.992-6.428l-5.01 1.675 1.619 4.828 5.011-1.674-1.62-4.829z"></path></svg> -->
      <label for="company" class="label text-2xl">The Great Shangri-La Energy Company</label>
      <label for="company_tag" class="label p-0">Energy for the people. Energy by the people.</label>
    </div> 
    <div>
      <!-- <div class="grid grid-flow-col gap-4">
        <a><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="fill-current"><path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"></path></svg></a> 
        <a><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="fill-current"><path d="M19.615 3.184c-3.604-.246-11.631-.245-15.23 0-3.897.266-4.356 2.62-4.385 8.816.029 6.185.484 8.549 4.385 8.816 3.6.245 11.626.246 15.23 0 3.897-.266 4.356-2.62 4.385-8.816-.029-6.185-.484-8.549-4.385-8.816zm-10.615 12.816v-8l8 3.993-8 4.007z"></path></svg></a> 
        <a><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" class="fill-current"><path d="M9 8h-3v4h3v12h5v-12h3.642l.358-4h-4v-1.667c0-.955.192-1.333 1.115-1.333h2.885v-5h-3.808c-3.596 0-5.192 1.583-5.192 4.615v3.385z"></path></svg></a>
      </div> -->
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