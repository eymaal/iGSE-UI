<script>
    import { onMount } from 'svelte';

    let readingDate = document.querySelector('#readingDate');
    // readingDate.nodeValue=new Date();
    let dayReading, nightReading;

    // onMount(() => {readingDate = new Date();})
    const newReading = document.querySelector('#newReading');

    function addReading(){
        alert('new Reading');
    }
</script>

<div class="h-screen w-screen bg-base-200 place-content-center flex">
    <div class="hero-content flex-col lg:flex-row-reverse w-full">
        <h1 class="text-2xl">Enter new Reading Details</h1>
        <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
            <div class="card-body">
                <form on:submit|preventDefault={addReading} id="newReading">
                    <div class="form-control">
                        <label for="readingDate" class="label">
                        <span class="label-text">Enter reading date</span>
                        </label>
                        <input type="date" bind:value={readingDate} class="input input-bordered" id="readingDate"/>
                    </div>

                    <div class="form-control">
                        <label for="dayReading" class="label">
                        <span class="label-text">Enter meter reading - day</span>
                        </label>
                        <label class="input-group">
                        <input type="number" bind:value={dayReading} class="input input-bordered grow" placeholder="Enter Day time Units" min=0/>
                        <span class="bg-neutral-content">kWh</span>
                        </label>
                    </div>

                    <div class="form-control">
                        <label for="nightReading" class="label">
                        <span class="label-text">Enter meter reading - night</span>
                        </label>
                        <label class="input-group">
                        <input type="number" bind:value={nightReading} class="input input-bordered grow" placeholder="Enter Night time Units" min=0/>
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
