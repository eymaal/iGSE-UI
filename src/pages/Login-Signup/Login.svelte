<script>
  import { push } from 'svelte-spa-router';
  import { fly } from 'svelte/transition';
  import Messagestore from '../../MessageStore';

  const login_description ="Login to view Energy usage and add new meter reading.";
  let customer_id = localStorage.getItem('lastLogin');
  let password_hash = "";
  let email_text = "";
  let pass_text = "";

  let msg={
    content: "",
    type: "",
    action: "",
    path: ""
  }
  
  async function login(){
    if(customer_id.length==0 || password_hash.length==0){
      if(customer_id.length==0) email_text = "Enter email id";
      if(password_hash.length==0) pass_text = "Enter password";
      return;
    }

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
      }
      return response.json();
    })
    .then((response) => {
      if(response.message){
        msg.content = response.message;
        email_text = " ";
        pass_text = msg.content;
      }
      if(response.customer_id){
        localStorage.setItem('customer', JSON.stringify(response));
        push('/dashboard');
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
    <div class="hero-content flex-col lg:flex-row-reverse" transition:fly="{{ y: 100, duration: 200 }}">
      <div class="text-center lg:text-left">
        <h1 class="text-5xl font-bold">iGSE</h1>
        <h4 class="text-2xl ">An Energy Tool</h4>
        <p class="py-6">{login_description}</p>
      </div>
      <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
        <div class="card-body">
          <form on:submit|stopPropagation|preventDefault={login} class="contents">
            <div class="form-control">
              <label for="email" class="label">
                <span class="label-text">Email</span>
              </label>
              <input type="text" placeholder="email" class="input input-bordered {email_text.length>0? 'input-error':''}" name="email" bind:value={customer_id}/>
              <span class="label-text-alt">{email_text}</span>
            </div>
            <div class="form-control">
              <label for="password" class="label">
                <span class="label-text">Password</span>
              </label>
              <input type="password" placeholder="password" class="input input-bordered {pass_text.length>0? 'input-error':''}" name="password" bind:value={password_hash}/>
              <span class="label-text-alt text-error">{pass_text}</span>
            </div>
            <div class="form-control">
              <label for="registration" class="label justify-end">
                <a href="#/registration" class="label-text-alt link link-hover">Not registered yet? Click here.</a>
              </label>
            </div>
            <div class="form-control">
              <button type="submit" class="btn btn-primary">Login</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>


<style>
  .input-error::placeholder {
    color: hsl(var(--er));
  }
</style>