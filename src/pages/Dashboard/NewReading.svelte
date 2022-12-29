<script>
    import { onMount } from 'svelte';
    import {pop} from 'svelte-spa-router';

    let date = new Date();
    let today = date.getFullYear().toString().padStart(4, '0') + '-' + (date.getMonth()+1).toString().padStart(2, '0') + '-' + date.getDate().toString().padStart(2, '0');
    let readingDate = today;
    // readingDate.nodeValue=new Date();
    let dayReading, nightReading, gasReading;

    // onMount(() => {readingDate = new Date();})
    const newReading = document.querySelector('#newReading');

    function addReading(){
        alert(readingDate);
    }

</script>

<div class="h-screen w-screen bg-base-200 place-content-center flex">
    <div class="hero-content flex-col lg:flex-row-reverse w-full">
        <button class="btn btn-circle btn-outline btn-md" on:click={()=> pop()}>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
        </button>
        <h1 class="text-2xl">Enter new Reading Details</h1>
        <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
            <div class="card-body">
                <form on:submit|preventDefault={addReading} id="newReading">
                    <div class="form-control">
                        <label for="readingDate" class="label">
                        <span class="label-text">Enter reading date</span>
                        </label>
                        <input required type="date" bind:value={readingDate} class="input input-bordered" id="readingDate"/>
                    </div>

                    <div class="form-control">
                        <label for="dayReading" class="label">
                        <span class="label-text">Enter Electricity reading - day</span>
                        </label>
                        <label class="input-group">
                        <input type="number" bind:value={dayReading} class="input input-bordered grow" placeholder="Enter Day time Units" min=0/>
                        <span class="bg-neutral-content">kWh</span>
                        </label>
                    </div>

                    <div class="form-control">
                        <label for="nightReading" class="label">
                        <span class="label-text">Enter Electricity reading - night</span>
                        </label>
                        <label class="input-group">
                        <input type="number" bind:value={nightReading} class="input input-bordered grow" placeholder="Enter Night time Units" min=0/>
                        <span class="bg-neutral-content">kWh</span>
                        </label>
                    </div>

                    <div class="form-control">
                        <label for="gasReading" class="label">
                        <span class="label-text">Enter Gas meter reading</span>
                        </label>
                        <label class="input-group">
                        <input type="number" bind:value={gasReading} class="input input-bordered grow" placeholder="Enter Gas Units" min=0/>
                        <span class="bg-neutral-content">kWh</span>
                        </label>
                    </div>

                    <div class="form-control">
                        <button class="btn btn-primary mt-5">Add Reading</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
