<script>
    import { onMount } from "svelte";

    let customers=0, unpaidBills=0;

    onMount(() => {
        getStat();
    })

    let getStat = async () => {
        const res = await fetch('http://localhost:8080/iGSE/admin/basicStats',{
            method: 'GET',
            headers: {
            'Content-Type': 'application/json'
            }
        })
        .then((Response) => {
            return Response.json();
        })
        .then((stats) => {
            customers = stats.customers;
            unpaidBills = stats.unpaidBills;
        })
    }
</script>

<div class="stats stats-horizontal shadow bg-primary text-primary-content">    
    <div class="stat">
        <div class="stat-figure text-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-primary-content"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path></svg>
        </div>
        <div class="stat-title">Customers</div>
        <div class="stat-value">{customers}</div>
        <div class="stat-desc">Total Customers</div>
    </div>    
    <div class="stat">
        <div class="stat-figure text-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-8 h-8 stroke-primary-content"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 8h14M5 8a2 2 0 110-4h14a2 2 0 110 4M5 8v10a2 2 0 002 2h10a2 2 0 002-2V8m-9 4h4"></path></svg>
        </div>
        <div class="stat-title">Unpaid Readings</div>
        <div class="stat-value">{unpaidBills}</div>
        <div class="stat-desc">↗︎</div>
    </div>    
    <!-- <div class="stat">
    <div class="stat-title">New Registers</div>
    <div class="stat-value">1,200</div>
    <div class="stat-desc">↘︎ 90 (14%)</div>
    </div>     -->
</div>