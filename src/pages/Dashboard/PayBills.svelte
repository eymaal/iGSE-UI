<script>
    import { push, pop } from 'svelte-spa-router';
    import { fly } from 'svelte/transition';
    import { onMount } from 'svelte';
    import Messagestore from '../../MessageStore';
    
    let msg = {
        content: "",
        type: "",
        action: "",
        path: ""
    }

    let disabled;
    let customer_id;
    let balance = 0;
    let dues = 0.0;
    let recharge = () => {push('/recharge')};
    let pay = async () => {
        const res = await fetch('http://localhost:8080/iGSE/payBill?customer_id='+customer_id, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json'
            }
        })
        .then((response) => {
            if(!response.ok){
                msg.type = "alert-error";
            } else {
                msg.content = "Payment Successful";
                msg.type = "alert-success";
            }
            return response.json();
        })
        .then((response) => {
            if(response.message){
                msg.content = response.message;
            }
            if(response.customer_id){
                localStorage.setItem('customer', JSON.stringify(response));
                setTimeout(() => {push('/dashboard')}, 1000);
            }
        })
        .finally(() => {
            Messagestore.update(() => msg);
        })
    }

    onMount(async () => {
        if(!localStorage.getItem('customer')){
            push('/login');
        } else{
            if(JSON.parse(localStorage.getItem('customer')).type=='admin'){
                push('/admin');
            }
            balance = JSON.parse(localStorage.getItem('customer')).balance;
        }

        customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
        const res = await fetch('http://localhost:8080/iGSE/calculateBill?customer_id='+customer_id, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json'
            }
        })
        .then((response) => {
            return response.json();
        })
        .then((response) => {
            dues = response.amount;
        })
    })
    
    
</script>

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col" transition:fly="{{ y: 100, duration: 200 }}">
        <button class="btn btn-circle btn-outline btn-md" on:click={pop}>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
        </button>  
        <div class="text-center lg:text-left">
            <h1 class="text-5xl font-bold">Bill Payment</h1>
        </div>
        <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
            <div class="card-body">
                <div class="form-control">
                    <label for="balance" class="label">
                        <span class="label-text">Current Wallet Balance:</span>
                    </label>
                    <label class="input-group">
                        <span>£</span>
                        <input
                            type="text"
                            placeholder="0.01"
                            class="input input-bordered w-full"
                            disabled
                            bind:value={balance}
                        />
                    </label>
                </div>
                <div class="form-control">
                    <label for="dues" class="label">
                        <span class="label-text">Bill Amount Pending:</span>
                    </label>
                    <label class="input-group">
                        <span>£</span>
                        <input
                            type="text"
                            placeholder="0.01"
                            class="input input-bordered w-full"
                            disabled
                            bind:value={dues}
                        />
                    </label>
                </div>
                {#if balance<dues}
                    <span class = "text-sm text-error text-center mt-3">Dues exceed current balance.<br/>Recharge balance to pay bill.</span>
                {/if}
                {#if dues==0}
                    <span class = "text-sm text-warning text-center mt-3">No dues. Add a reading to pay.</span>
                {/if}
                <div class="form-control mt-3">
                {#if balance>=dues}
                    {#if dues ==0}
                        <button class="btn btn-primary" on:click={pay} disabled>Pay Now</button>
                    {:else}
                        <button class="btn btn-primary" on:click={pay}>Pay Now</button>
                    {/if}
                {:else}
                    <button class="btn btn-primary" on:click={recharge}>Add Voucher</button>
                {/if}
                </div>
            </div>
        </div>
    </div>
</div>
