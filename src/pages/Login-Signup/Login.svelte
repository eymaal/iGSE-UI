<script>
  const login_description ="Login to view Energy usage and add new meter reading.";
  import {push, pop, replace} from 'svelte-spa-router';
  let email;
  let password;
  
  function login(){
      alert(email + password);
      push('/dashboard');
  }
</script>

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col lg:flex-row-reverse">
      <div class="text-center lg:text-left">
        <h1 class="text-5xl font-bold">iGSE</h1>
        <h4 class="text-2xl ">An Energy Tool</h4>
        <p class="py-6">{login_description}</p>
      </div>
      <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
        <div class="card-body">
          <div class="form-control">
            <label for="email" class="label">
              <span class="label-text">Email</span>
            </label>
            <input type="text" placeholder="email" class="input input-bordered" name="email" bind:value={email}/>
          </div>
          <div class="form-control">
            <label for="password" class="label">
              <span class="label-text">Password</span>
            </label>
            <input type="password" placeholder="password" class="input input-bordered" name="password" bind:value={password}/>
            <label for="forget-password" class="label">
              <!-- svelte-ignore a11y-invalid-attribute -->
              <a href="#" class="label-text-alt link link-hover">Forgot password?</a>
            </label>
          </div>
          <div class="form-control">
            <label for="registration" class="label justify-end">
              <a href="#/registration" class="label-text-alt link link-hover">Not registered yet? Click here.</a>
            </label>
          </div>
          <div class="form-control">
            <button on:click|stopPropagation={login} class="btn btn-primary">Login</button>
          </div>
        </div>
      </div>
    </div>
  </div>