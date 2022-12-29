<script>
  
  import * as yup from 'yup';
  import {Form, Message, isInvalid} from 'svelte-yup';
  import Messages from '../../util/Messages.svelte';
  
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
            console.log(message);
          }
        })
      .catch(() => {
        messages.type = "alert-error";
        messages.content = "Request cannot be sent";
      });

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

<Messages {...messages}/>
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
            <input type="text" placeholder="Enter 8 digit code" class="input input-bordered" id="EVC_code" bind:value={customer.EVC_code} class:invalid={invalid("EVC_code")}/>
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