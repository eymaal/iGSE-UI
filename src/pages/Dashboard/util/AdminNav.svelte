<script>
    import { push } from 'svelte-spa-router';

    let logout = () => {
        if (localStorage.getItem("customer")) {
			localStorage.setItem(
				"lastLogin",
				JSON.parse(localStorage.getItem("customer")).customer_id
			);
			localStorage.removeItem("customer");
		}
		push("/login");
    };
</script>

<div class="navbar bg-base-100 fixed top-0 z-50">
    <div class="flex-none">
        <button class="btn btn-square btn-ghost lg:hidden">
            <label for="my-drawer-2" class="drawer-button w-full h-full flex justify-center items-center cursor-pointer">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-5 h-5 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg>                
            </label>
        </button>
    </div>
    <div class="flex-1">
        <!-- svelte-ignore a11y-missing-attribute -->
        <a class="btn btn-ghost normal-case text-xl">iGSE - Admin</a>
    </div>
    <div class="flex-none">
        <button class="btn btn-square btn-ghost" on:click={logout}>
            <!-- <svg class = "inline-block w-6 h-6 stroke-current" viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg" fill="none"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"><title>ionicons-v5-o</title><path d="M304,336v40a40,40,0,0,1-40,40H104a40,40,0,0,1-40-40V136a40,40,0,0,1,40-40H256c22.09,0,48,17.91,48,40v40" style="fill:none;stroke:#000000;stroke-linecap:round;stroke-linejoin:round;stroke-width:32px"></path><polyline points="368 336 448 256 368 176" style="fill:none;stroke:#000000;stroke-linecap:round;stroke-linejoin:round;stroke-width:32px"></polyline><line x1="176" y1="256" x2="432" y2="256" style="fill:none;stroke:#000000;stroke-linecap:round;stroke-linejoin:round;stroke-width:32px"></line></g></svg> -->
            <!-- <svg xmlns="http://www.w3.org/2000/svg" class = "inline-block w-6 h-6 stroke-current" viewBox="0 0 24 24" fill="none" stroke="#000000" stroke-width="1.416" stroke-linecap="round" stroke-linejoin="round"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"> <path d="M9 21H5a2 2 0 01-2-2V5a2 2 0 012-2h4"></path> <polyline points="16 17 21 12 16 7"></polyline> <line x1="21" y1="12" x2="9" y2="12"></line> </g></svg> -->
            <svg xmlns="http://www.w3.org/2000/svg" class = "inline-block w-6 h-6 stroke-current" viewBox="0 0 24 24" fill="none" stroke="#000000" stroke-width="1.6799999999999997" stroke-linecap="round" stroke-linejoin="round"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_iconCarrier"> <path d="M9 21H5a2 2 0 01-2-2V5a2 2 0 012-2h4"></path> <polyline points="16 17 21 12 16 7"></polyline> <line x1="21" y1="12" x2="9" y2="12"></line> </g></svg>
        </button>
    </div>
</div>