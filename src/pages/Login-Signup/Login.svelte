<script>
  import { push } from 'svelte-spa-router';
  import Messagestore from '../../MessageStore';

  const login_description ="Login to view Energy usage and add new meter reading.";
  let customer_id = localStorage.getItem('lastLogin');
  let password_hash = "";

  let msg={
    content: "",
    type: "",
    action: "",
    path: ""
  }
  
  async function login(){
    const res = await fetch('http://localhost:8080/iGSE/login',{
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({customer_id,password_hash})
    })
    .then((response) => {
      if(!response.ok){
        msg.type = "alert-error";
      } else {
        msg.content = "";
        msg.type = "";
        push('/dashboard');          
      }
      return response.json();
    })
    .then((response) => {
      if(response.message){
        msg.content = response.message;
      }
      if(response.customer_id){
        localStorage.setItem('customer', JSON.stringify(response));
      }
    })
    .catch((error) => {
      console.log(error);
      msg.type = "alert-error";
      msg.content = "Request cannot be sent";
    })
    .finally(() => {
      Messagestore.update(() => msg);
    })
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
            <input type="text" placeholder="email" class="input input-bordered" name="email" bind:value={customer_id}/>
          </div>
          <div class="form-control">
            <label for="password" class="label">
              <span class="label-text">Password</span>
            </label>
            <input type="password" placeholder="password" class="input input-bordered" name="password" bind:value={password_hash}/>
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