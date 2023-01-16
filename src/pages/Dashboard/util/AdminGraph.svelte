<script>
    import { onMount } from "svelte";

    let customer_id;

    onMount(() => {
        customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
        getConsumptionData();
    })

    let getConsumptionData = async () => {
        const res = await fetch('http://localhost:8080/iGSE/admin/stats?customer_id='+customer_id,{
            method: 'GET',
            headers: {
            'Content-Type': 'application/json'
            }
        })
        .then((response) => {
            return response.json();
        })
        .then((data) => {
            console.log(data);
        })
    }

    
    
</script>

<div id="chart"></div>
