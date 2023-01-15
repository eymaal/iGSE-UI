<script>
    import { Html5Qrcode } from 'html5-qrcode';
    import { onMount, onDestroy } from 'svelte';
    import { push, pop } from 'svelte-spa-router';
    import { fly } from 'svelte/transition';

    let scanning = false;
    let modal;
    let EVC_code = "";
    let title = "";
    let description = "Click Confirm to redeem Voucher for £200, or Cancel to scan another code.";
    let submit = true;
    let complete = false;
    let secondaryButtonText = "Cancel";
    let inputClass = "input input-bordered flex-grow";

    let html5Qrcode;

    $: {
        // @ts-ignore
        resetModal() || EVC_code;
    }

    let resetModal = () => {
        submit = true;
        secondaryButtonText = "Cancel";
        description = "Click Confirm to redeem Voucher for £200, or Cancel to scan another code.";
    }

    onMount(() => {
        if(!localStorage.getItem('customer')){
            push('/login');
        } else{
            if(JSON.parse(localStorage.getItem('customer')).type=='admin'){
                push('/admin');
            }
        }
        init();
    });
    onDestroy(() =>{
        if(scanning){
            stop();
        }
    })

    function init() {
        html5Qrcode = new Html5Qrcode('reader');
    }

    function start() {
        submit = true;
        secondaryButtonText = "Cancel";
        description = "Click Confirm to redeem Voucher for £200, or Cancel to scan another code.";
        html5Qrcode.start(
            { facingMode: 'environment' },
            {
                fps: 10,
                qrbox: { width: 250, height: 250 },
            },
            onScanSuccess,
            onScanFailure
        );
        scanning = true;
    }

    async function stop() {
        await html5Qrcode.stop();
        scanning = false;
    }

    function close() {
        if(scanning){
            stop();
        }
        pop();
    }

    function setThingsInMotion(){
        if(EVC_code.length==0){
            inputClass = "input input-bordered flex-grow input-error";
        } else {
            inputClass = "input input-bordered flex-grow";
            title = `Confirm ${EVC_code}`;
            modal.click();
        }
    }

    function onScanSuccess(decodedText, decodedResult) {
        EVC_code = decodedText;
        setThingsInMotion();
        stop();
        console.log(decodedResult);
    }

    function onScanFailure(error) {
        console.warn(`Code scan error = ${error}`);
    }

    async function addCode() {
        let customer = JSON.parse(localStorage.getItem('customer'));
        const res = await fetch('http://localhost:8080/iGSE/addVoucher?evcCode='+ EVC_code,{
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(customer)
        })
        .then((response) => {
            if(!response.ok){
                title = "Error";
            } else {
                title = "Success";
            }
            return response.json();
        })
        .then((response) => {
            submit = false;
            if(response.message){
                description = response.message;
                secondaryButtonText = "Scan Again";
            } else {
                description = `Voucher has been redeemed. Your updated balance: ${response.balance}`;
                let customer = JSON.parse(localStorage.getItem('customer'));
                customer.balance = response.balance;
                localStorage.setItem('customer', JSON.stringify(customer));
                complete = true;
            }
        })
    }

</script>

<style>
    reader {
        width: auto;
        min-width: 80%;
        height: auto;
        min-height: 300px;
        background-color: black;
    }
</style>

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col lg:flex-row-reverse gap-3 p-8 w-full max-w-4xl" transition:fly="{{ y: 100, duration: 200 }}">
        <button class="btn btn-circle btn-outline btn-md lg:hidden" on:click={close}>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
        </button>   
        <!-- svelte-ignore a11y-media-has-caption -->
        <reader id="reader" class="flex justify-center"/>
        <div class = "hero-content flex flex-col">
            <button class="btn btn-circle btn-outline btn-md hidden lg:flex" on:click={close}>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
            </button> 
            {#if scanning}
                <button class="btn btn-primary" on:click={stop}>Stop Scanning</button>
            {:else}
                <button class="btn btn-primary" on:click={start}>Start Scanning</button>
            {/if}
            <div class="divider divider-horizontal">OR</div>
            <div class="form-control">
                <div class="input-group">
                  <input type="text" placeholder="Enter 8 digit code" class={inputClass} id="EVC_code" bind:value={EVC_code}/>
                  <button class="btn" on:click={setThingsInMotion}>✔</button>
                </div>
            </div>
            <div class = "w-max flex items-center flex-col">
                <h1 class="text-3xl font-bold">Recharge</h1>
                <p class="py-6">Scan the QR Code on your E-voucher</p>
            </div>
        </div>
    </div>
</div>

<label for="my-modal" class="btn hidden">Confirmation Modal</label>

<!-- Put this part before </body> tag -->
<input type="checkbox" id="my-modal" class="modal-toggle" bind:this={modal}/>
<div class="modal">
  <div class="modal-box">
    <h3 class="font-bold text-lg">{title}</h3>
    <p class="py-4">{description}</p>
    <div class="modal-action">
        {#if submit}
            <button class="btn btn-primary" on:click={addCode}>Confirm</button> 
        {/if}
        {#if complete}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <label for="my-modal" class="btn btn-primary" on:click={close}>OK</label>
        {:else}
            <label for="my-modal" class="btn">{secondaryButtonText}</label>
        {/if}
    </div>
  </div>
</div>