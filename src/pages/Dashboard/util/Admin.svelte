<script>
    import AdminNav from "./AdminNav.svelte";
    import AdminReading from "./AdminReading.svelte";
    import AdminStat from "./AdminStat.svelte";
    import Footer from "./Footer.svelte";
    import { push } from 'svelte-spa-router';
    import { onMount } from "svelte";
    import AdminGraph from "./AdminGraph.svelte";

    let rates = [], rates_backup = [];
    let customer_id;
    let modalText = "Cancel";
    let modalPrimary = "Update";

    onMount(() => {
        if(!localStorage.getItem('customer')){
            push('/login');
        } else {
            if(JSON.parse(localStorage.getItem('customer')).type=='customer'){
                push('/dashboard');
            }
        }
        customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
        getRates();
    })

    let getRates = async () => {
        const res = await fetch('http://localhost:8080/iGSE/admin/rates?customer_id='+customer_id,{
            method: 'GET',
            headers: {
            'Content-Type': 'application/json'
            }
        })
        .then((response) =>{
            return response.json();
        })
        .then((rateList) => {
            rates_backup = rateList;
            resetRates();
            console.log(rates);
        })
    }

    function resetRates(){
        rates = rates_backup;
        modalPrimary = "Update";
        modalText = "Cancel";
    }

    let updateRates = async () => {
        const res = await fetch('http://localhost:8080/iGSE/admin/setrates?customer_id='+customer_id,{
            method: 'PUT',
            headers: {
            'Content-Type': 'application/json'
            },
            body: JSON.stringify(rates)
        })
        .then((response) => {
            return response.json();
        })
        .then((rateList) => {
            rates_backup = rateList;
            resetRates();
            modalText = "OK";
            modalPrimary = "";

        })
    }

</script>

<AdminNav/>
<div class="drawer drawer-mobile mt-16 overflow-hidden">
    <input id="my-drawer-2" type="checkbox" class="drawer-toggle" />
    <div class="drawer-content block items-center justify-center lg:ml-80">
        <!-- Page content here -->
        <div class="flex flex-col gap-2 min-h-screen items-center justify-center mt-16">
            <AdminStat/>

            <AdminGraph/>
            
            <AdminReading/>
        </div>
        <Footer/>
        <!-- Page content ends-->
        
    
    </div> 
    <div class="drawer-side mt-16 top-0 absolute">
        <label for="my-drawer-2" class="drawer-overlay"></label> 
        <ul class="menu p-4 w-80 bg-base-100 text-base-content">
            <!-- Sidebar content here -->
            <li><a  class ="p-0"><label for="my-modal-6" class="h-full w-full pr-4 pl-4 pt-3 pb-3 cursor-pointer">Set Unit Rates</label></a></li>
            <!-- <li><a>Sidebar Item 2</a></li> -->
        </ul>
    
    </div>
</div>

<input type="checkbox" id="my-modal-6" class="modal-toggle" />
<div class="modal modal-bottom sm:modal-middle">
    <div class="modal-box">
        <h3 class="font-bold text-lg">Update Unit Rates</h3>
        <div class="modal-action flex-col">
            <form on:submit={updateRates}>
                <div class="rate-container mb-3">
                    {#each rates as rate}
                        <div class="form-control">
                            <label class="label" for={rate.tariff_type}>
                                <span class="label-text">{rate.tariff_type}</span>
                            </label>
                            <label for={rate.tariff_type} class="input-group">
                                <span>£</span>
                                <input type="number" placeholder="Enter rate" class="input input-bordered w-full" bind:value={rate.rate} min=0 step="any"/>
                            </label>
                        </div>                
                    {/each}
                </div>
                <div class="btn-container flex justify-end gap-1">
                    {#if modalPrimary.length>0}                    
                        <button type = "submit" class="btn btn-primary">{modalPrimary}</button>
                    {/if}
                    <!-- svelte-ignore missing-declaration -->
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <label for="my-modal-6" class="btn" on:click={resetRates}>{modalText}</label>
                </div>
            </form>
        </div>
    </div>
</div>

<style>
    .drawer,
    .drawer-side {
        height: calc(100vh - 4rem);
    }

    .rate-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(186px, 1fr));
        gap: 0.5rem;
    }
</style>