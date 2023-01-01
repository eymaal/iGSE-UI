<script>
    import AdminNav from "./AdminNav.svelte";
    import AdminReading from "./AdminReading.svelte";
    import AdminStat from "./AdminStat.svelte";
    import Footer from "./Footer.svelte";
    import { push } from 'svelte-spa-router';
    import { onMount } from "svelte";

    onMount(() => {
        if(!localStorage.getItem('customer')){
            push('/login');
        } else {
            if(JSON.parse(localStorage.getItem('customer')).type=='customer'){
                push('/dashboard');
            }
        }
    })

</script>

<AdminNav/>
<div class="drawer drawer-mobile mt-16 overflow-hidden">
    <input id="my-drawer-2" type="checkbox" class="drawer-toggle" />
    <div class="drawer-content flex flex-col items-center justify-center lg:ml-80">
        <!-- Page content here -->
        <div class="flex flex-col gap-2 min-h-screen items-center justify-center mt-16">
            <AdminStat/>
            <AdminReading/>
        </div>
        <Footer/>
        <!-- Page content ends-->
        
    
    </div> 
    <div class="drawer-side mt-16 top-0 absolute">
        <label for="my-drawer-2" class="drawer-overlay"></label> 
        <ul class="menu p-4 w-80 bg-base-100 text-base-content">
            <!-- Sidebar content here -->
            <li><a>Set Unit Rates</a></li>
            <li><a>Sidebar Item 2</a></li>
        </ul>
    
    </div>
</div>

<style>
    .drawer,
    .drawer-side{
        height: calc(100vh - 4rem);
    }
</style>