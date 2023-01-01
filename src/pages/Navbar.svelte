<script>
	import { push } from "svelte-spa-router";
    import ThemeChange from "./Dashboard/util/ThemeChange.svelte";
	
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
	let balance = 0;
	if(localStorage.getItem('customer')){
		balance = JSON.parse(localStorage.getItem("customer")).balance;
	}
</script>

<div class="navbar bg-base-100">
	<div class="navbar-start">
		<div class="dropdown">
			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<label
				for="hamburger"
				tabindex="0"
				class="btn btn-ghost btn-circle"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-5 w-5"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
					><path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M4 6h16M4 12h16M4 18h7"
					/></svg
				>
			</label>

			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<ul
				tabindex="0"
				class="menu menu-compact lg:menu-normal dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52"
			>
				<li><a href="/#/addReading">Add Reading</a></li>
				<li><a href="/#/recharge">Add E-Voucher</a></li>
				<li><a href="/#/billpay">Pay Bills</a></li>
				<li><a href="/#/login">Log Out</a></li>
			</ul>
		</div>
	</div>
	<div class="navbar-center">
		<a href="/#/Dashboard" class="btn btn-ghost normal-case text-xl">iGSE</a
		>
	</div>
	<div class="navbar-end">
		<ThemeChange/>

		<div class="dropdown dropdown-end">
			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<label for="profile" tabindex="0" class="btn btn-ghost btn-circle">
				<svg
					stroke="currentColor"
					fill="currentColor"
					class="h-5 w-5"
					viewBox="0 0 32 32"
					xmlns="http://www.w3.org/2000/svg"
					><g id="SVGRepo_bgCarrier" stroke-width="0" /><g
						id="SVGRepo_iconCarrier"
					>
						<title />
						<g id="about">
							<path
								d="M16,16A7,7,0,1,0,9,9,7,7,0,0,0,16,16ZM16,4a5,5,0,1,1-5,5A5,5,0,0,1,16,4Z"
							/>
							<path
								d="M17,18H15A11,11,0,0,0,4,29a1,1,0,0,0,1,1H27a1,1,0,0,0,1-1A11,11,0,0,0,17,18ZM6.06,28A9,9,0,0,1,15,20h2a9,9,0,0,1,8.94,8Z"
							/>
						</g>
					</g></svg
				>
			</label>

			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<ul
				tabindex="0"
				class="lg:menu-normal menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-100 rounded-box w-52"
			>
				<li><span>Balance: {balance}</span></li>
				<!-- svelte-ignore a11y-invalid-attribute -->
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-missing-attribute -->
				<li><a on:click={logout}>Log out</a></li>
			</ul>
		</div>
	</div>
</div>
