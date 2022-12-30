<script>
// @ts-nocheck
  import * as yup from 'yup';
  import { Form, Message, isInvalid } from 'svelte-yup';
  import { push } from 'svelte-spa-router';
  import Messagestore from '../../MessageStore';
  
  let messages={
    content: "",
    type: "",
    action: "",
    path: ""
  }

  let schema = yup.object().shape({
    customer_id: yup.string().required().email().label("Email Address"),
    password_hash: yup.string().required().label("Password"),
    address: yup.string().required().label("Address"),
    property_type: yup.string().required().label("Property Type"),
    bedroom_num: yup.number().required().min(1).label("Number of Bedrooms").nullable(true).transform((v, o) => o === '' ? null : v),
    EVC_code: yup.string().required().length(8).label("Energy Voucher Code")
  });

  let customer = {
    customer_id: "",
    password_hash: "",
    property_type: "",
    address: "",
    bedroom_num: 0,
    EVC_code: ""
  }

  let submitted = false;
  let isValid;
  async function register(){
    submitted = true;
    isValid = schema.isValidSync(customer);
    if(isValid){
      const res = await fetch('http://localhost:8080/iGSE/register?evcCode='+customer.EVC_code,{
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(customer)
      })
      .then((response) => {
        
        if(!response.ok){
          messages.type = "alert-error";
          return response.json();          
        } else{
          messages.type = "alert-success";
          messages.content = "Registration Successful! Click here to log in.";
          messages.path = "/login";
          return response.json();          
        }}).then((message) => {
          if(messages.type=="alert-error"){
            messages.content=message.message;            
          }
        })
      .catch(() => {
        messages.type = "alert-error";
        messages.content = "Request cannot be sent";
      })
      .finally(() => {
        Messagestore.update(currentMessage => {
          return messages;
        });
      })

    }
  }

  $: invalid = (name)=>{
    if(submitted){
        return isInvalid(schema, name, customer);
    }
    return false;
  }

  let property_types = [
    'detached',
    'terrace',
    'cottage',
    'semi-detached',
    'flat',
    'bunglow',
    'mansion',
  ]

</script>

<div class="hero min-h-screen bg-base-200">
  <div class="hero-content flex-col lg:flex-col">
    <div class="text-center lg:text-left">
      <h1 class="text-5xl font-bold">Get Started!</h1>
      <p class="py-6">Enter your details and become a part of the Great Shangri-La Energy Company!</p>
      <a href="#/login" class="label-text-alt link link-hover">Already Registered? Click to Log in.</a>
    </div>

    <Form id="registration" class="contents form" {schema} fields={customer} submitHandler={register} {submitted} color="hsl(var(--er))">
      <div class="card flex-shrink-0 w-full max-w-sm shadow-2xl bg-base-100">
        <div class="card-body">

          <div class="form-control">
            <label for="customer_id" class="label">
              <span class="label-text">Email Address</span>
            </label>
            <input type="text" placeholder="Enter your email address" class="peer input input-bordered" id="customer_id" bind:value={customer.customer_id} class:invalid={invalid("customer_id")}/>
            <Message name="customer_id"/>
          </div>

          <div class="form-control">
            <label for="password" class="label">
              <span class="label-text">Password</span>
            </label>
            <input type="password" placeholder="Enter your password" class="input input-bordered" id="password_hash" bind:value={customer.password_hash} class:invalid={invalid("password_hash")}/>
            <Message name="password_hash"/>
          </div>
      
          <div class="form-control">
              <label for="address" class="label">
                <span class="label-text">Address</span>
              </label>
              <input type="text" placeholder="Enter your address" class="input input-bordered" id="address" bind:value={customer.address} class:invalid={invalid("address")}/>
              <Message name="address"/>
          </div>

          <div class="form-control">
            <label for="property_type" class="label">
              <span class="label-text">Property Type</span>
            </label>
            <select class="select select-bordered w-full max-w-xs font-normal" bind:value={customer.property_type} class:invalid={invalid("property_type")}>
              <option disabled selected>Select your property type</option>
              {#each property_types as type}
                <option value="{type}">{type}</option>
              {/each}
            </select>
            <Message name="property_type"/>
          </div>

          <div class="form-control">
            <label for="bedrooms" class="label">
              <span class="label-text">Number of Bedrooms</span>
            </label>
            <input type="text" placeholder="Enter number of bedrooms" class="input input-bordered" id="bedroom_num" min=0 bind:value={customer.bedroom_num} class:invalid={invalid("bedroom_num")}/>
            <Message name="bedroom_num"/>
          </div>

          <div class="form-control">
            <label for="evoucher" class="label">
              <span class="label-text">Energy Voucher code</span>
            </label>
            <div class="input-group">
              <input type="text" placeholder="Enter 8 digit code" class="input input-bordered flex-grow" id="EVC_code" bind:value={customer.EVC_code} class:invalid={invalid("EVC_code")}/>
              <button class="btn btn-square" on:click|stopPropagation={() => {push("/recharge")}}>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 64 64" stroke="currentColor" stroke-width="4"><g id="SVGRepo_bgCarrier" stroke-width="3.17"></g><g id="SVGRepo_iconCarrier"><path d="M54.1,50.13H9.9a2,2,0,0,1-2-2V21.87c0-1.11.89-1,2-1h7.54a1,1,0,0,0,.76-.35l4.1-5.95a2,2,0,0,1,1.52-.7H40a2,2,0,0,1,1.51.7l4.11,5.95a1,1,0,0,0,.76.35H54.1c1.11,0,2-.11,2,1V48.13A2,2,0,0,1,54.1,50.13Z"></path><circle cx="32" cy="34" r="10.05"></circle></g></svg>
              </button>
            </div>
            <Message name="EVC_code"/>
          </div>
      
      
          <div class="form-control mt-6">
            <button class="btn btn-primary" type="submit">Register</button>
          </div>
        </div>
      </div>
    </Form>
  </div>
</div>

<style>
  .invalid {
    @apply input-error;
  }
  .invalid {
    @apply p-4; 
  }
</style>