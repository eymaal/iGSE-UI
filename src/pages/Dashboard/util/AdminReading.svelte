<script>
    import { onMount } from "svelte";

    let customer_id;
    let readings = [];

    onMount(async () => {
        customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
        const res = await fetch('http://localhost:8080/iGSE/admin/readings?customer_id='+customer_id,{
            method: 'GET',
            headers: {
                'Content-Type': 'application/json'
            }
        })
        .then((response) => {
            if(!response.ok){
                readings = [];
            }
            return response.json();
        })
        .then((result) => {
            readings = result;
        })
    })
</script>

{#if readings.length>0}
    <div class="overflow-x-auto span-all-columns w-full flex justify-center">
      <table class="table table-compact w-full">
        <thead>
          <tr>
            <th></th>
            <th>Customer</th>
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
              <td>{reading.customer_id}</td>
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
      No readings yet.
    </span>
  </p>
  {/if}

  <style>
    .table thead th{
        @apply bg-primary;
        color: hsl(var(--b1));
    }
  </style>