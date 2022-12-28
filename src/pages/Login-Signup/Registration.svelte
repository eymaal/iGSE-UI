<script>
  let result = null;
  let customer_id, password_hash, property_type, address, bedroom_num, EVC_code, balance=200.0, type="user";
  let property_types = [
    'detached',
    'terrace',
    'cottage',
    'semi-detached',
    'flat',
    'bunglow',
    'mansion',
  ]

  async function register(){
    const res = await fetch('http://localhost:8080/iGSE/register',{
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        customer_id,
        password_hash,
        address,
        property_type,
        bedroom_num,
        balance,
        type
      })
    })
    
    const json = await res.json();
    result = JSON.stringify(json);
    alert(result);
  }

  function onUpdate(event){
    console.log(event);
  }
</script>

<div class="hero min-h-screen bg-base-200">
  <div class="hero-content flex-col lg:flex-col">
    <div class="text-center lg:text-left">
      <h1 class="text-5xl font-bold">Get Started!</h1>
      <p class="py-6">Enter your details and become a part of the Great Shangri-La Energy Company!</p>
    </div>
    <form id="registration" class="contents" on:submit|preventDefault={register}>
      <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
        <div class="card-body">

          <div class="form-control">
            <label for="email" class="label">
              <span class="label-text">Email</span>
            </label>
            <input required type="email" placeholder="Enter your email address" class="input input-bordered required:border-red-500" name="email" bind:value={customer_id}/>
          </div>

          <div class="form-control">
            <label for="password" class="label">
              <span class="label-text">Password</span>
            </label>
            <input required type="password" placeholder="Enter your password" class="input input-bordered" name="password" bind:value={password_hash}/>
          </div>
      
          <div class="form-control">
              <label for="address" class="label">
                <span class="label-text">Address</span>
              </label>
              <input type="text" placeholder="Enter your address" class="input input-bordered" name="address" bind:value={address}/>
          </div>

          <div class="form-control">
            <label for="property_type" class="label">
              <span class="label-text">Property Type</span>
            </label>
            <select class="select select-bordered w-full max-w-xs font-normal" bind:value={property_type}>
              <option disabled selected>Select your property type</option>
              {#each property_types as type}
                <option value="{type}">{type}</option>
              {/each}
            </select>
          </div>

          <div class="form-control">
            <label for="bedrooms" class="label">
              <span class="label-text">Number of Bedrooms</span>
            </label>
            <input type="number" placeholder="Enter number of bedrooms" class="input input-bordered" name="bedrooms" min=0 bind:value={bedroom_num}/>
          </div>

          <div class="form-control">
            <label for="evoucher" class="label">
              <span class="label-text">Energy Voucher code</span>
            </label>
            <input type="text" placeholder="Enter 8 digit code" class="input input-bordered" name="evoucher" bind:value={EVC_code}/>
          </div>
      
      
          <div class="form-control mt-6">
            <button class="btn btn-primary" type="submit">Register</button>
          </div>
        </div>
      </div>
    </form>
  </div>
</div>