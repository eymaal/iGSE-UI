<script>
    import { Html5Qrcode } from 'html5-qrcode'
    import { onMount } from 'svelte'

    let scanning = false

    let html5Qrcode

    onMount(init)

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

<!-- <main class="bg-white p-10">
    <reader id="reader"/>
    {#if scanning}
        <button on:click={stop}>stop</button>
    {:else}
        <button on:click={start}>start</button>
    {/if}
</main> -->

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col lg:flex-row-reverse gap-3 p-8 w-full">
      <!-- svelte-ignore a11y-media-has-caption -->
      <reader id="reader" class="flex justify-center"/>
        {#if scanning}
            <button class="btn btn-primary" on:click={stop}>stop</button>
        {:else}
            <button class="btn btn-primary" on:click={start}>start</button>
        {/if}
      <div>
        <h1 class="text-3xl font-bold">Recharge</h1>
        <p class="py-6">Scan the QR Code on your E-voucher</p>
      </div>
    </div>
</div>
