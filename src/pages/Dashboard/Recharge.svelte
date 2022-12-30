<script>
    import { Html5Qrcode } from 'html5-qrcode';
    import { onMount } from 'svelte';
    import { pop } from 'svelte-spa-router';

    let scanning = false

    let html5Qrcode;
    onMount(init);

    function init() {
        html5Qrcode = new Html5Qrcode('reader')
    }

    function start() {
        html5Qrcode.start(
            { facingMode: 'environment' },
            {
                fps: 10,
                qrbox: { width: 250, height: 250 },
            },
            onScanSuccess,
            onScanFailure
        )
        scanning = true
    }

    async function stop() {
        await html5Qrcode.stop()
        scanning = false
    }

    function close() {
        if(scanning){
            stop();
        }
        pop();
    }

    function onScanSuccess(decodedText, decodedResult) {
        alert(`Code matched = ${decodedText}`)
        console.log(decodedResult)
    }

    function onScanFailure(error) {
        console.warn(`Code scan error = ${error}`)
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
    <div class="hero-content flex-col lg:flex-row-reverse gap-3 p-8 w-full">
        <button class="btn btn-circle btn-outline btn-md" on:click={close}>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
        </button>   
        <!-- svelte-ignore a11y-media-has-caption -->
        <reader id="reader" class="flex justify-center"/>
        <div>
            {#if scanning}
                <button class="btn btn-primary" on:click={stop}>Stop Scanning</button>
            {:else}
                <button class="btn btn-primary" on:click={start}>Start Scanning</button>
            {/if}
            <div>
                <h1 class="text-3xl font-bold">Recharge</h1>
                <p class="py-6">Scan the QR Code on your E-voucher</p>
            </div>
        </div>
    </div>
</div>