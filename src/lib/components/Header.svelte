<script lang="ts">
	import { Button, Search, Popover } from 'flowbite-svelte';
	import img from '$lib/images/logoikangud.svg';
	import calmwater from '$lib/images/Logo.svg';
	import defaultUser from '$lib/images/default_user.jpg';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	import { Hamburger } from 'svelte-hamburgers';
	import { fade } from 'svelte/transition';

	let open = false;

	function closeMenu() {
		open = false;
	}

	let options = {duration: 300};

	export let user: any;

	$: search = '';

	function searchFish() {
		$page.url.searchParams.set('search', search);
		goto(`/?${$page.url.searchParams.toString()}`);
	}

	const menus = [
		{
			title: 'PRODUCTS',
			href: '/products'
		},
		{
			title: 'ORDER',
			href: '/order',
			authenticated: true
		},
		{
			title: 'CART',
			href: '/cart'
		},
		{
			title: 'SELL',
			href: '/sell',
			authenticated: true
		}
	];
</script>

<!-- {#if open}
<nav class="lg:hidden absolute w-[100%] p-4 flex flex-col gap-2 items-center bg-red-500">
	{#each menus as menu}
		{#if !menu.authenticated || user}
			<li class="hover:text-gray-300">
				<a class="p-2" href={menu.href}>{menu.title}</a>
			</li>
		{/if}
	{/each}
</nav>
{/if} -->

<header
	class="flex justify-between items-center bg-[#724C9D]/90 h-[50px] lg:h-[76px] sticky top-0 text-lg text-white z-10"
>
	<a href="/" class="w-[35%] sm:w-[25%] lg:w-[15%] px-4">
		<img class="w-full" src={calmwater} alt="" />
	</a>

	<div class="hidden lg:w-[34%] lg:flex items-center gap-2 place-content-between">
		<Search class="py-2 flex items-center" placeholder="Search Items..." bind:value={search} />
		<Button on:click={searchFish} color="light">
			<svg
				class="w-5 h-5 mr-2 -ml-1"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
				/></svg
			>
			Search
		</Button>
	</div>

	<div class="flex">
		<!-- <button class="lg:hidden navbar-burger flex items-center text-white p-3">
			<svg
				class="block h-6 w-6"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M4 6h16M4 12h16m-7 6h7"
				/>
			</svg>
		</button> -->

		<div class="lg:hidden flex">
			<Hamburger --color="white" bind:open />
		</div>
		<!-- flex flex-col gap-4 p-4  -->

		<ul
			class="hidden lg:flex items-center gap-10 font-bold font-sans text-lg px-2 pr-4 min-[1150px]:pr-16"
		>
			{#each menus as menu}
				{#if !menu.authenticated || user}
					<li class="hover:text-gray-300">
						<a class="p-2" href={menu.href}>{menu.title}</a>
					</li>
				{/if}
			{/each}
		</ul>

		<ul class="flex items-center gap-10 font-bold font-sans text-lg pr-0 lg:pr-8">
			{#if user}
				<li class="h-full hover:text-gray-500">
					<Button btnClass="h-full bg-transparent">
						<img
							id="user"
							src={user.image ?? defaultUser}
							alt=""
							class="w-14 sm:w-14 lg:w-[80px] p-2 lg:p-4 rounded-full"
						/>
					</Button>
					<Popover
						triggeredBy="#user"
						placement="bottom"
						class="w-64 text-sm font-light text-gray-500 bg-white dark:text-gray-400 dark:border-gray-600 dark:bg-gray-800"
					>
						<div class="flex flex-col gap-4 p-3">
							<span class="font-semibold text-black text-lg">Welcome, {user.username}</span>
							<div class="h-[1px] bg-slate-800 w-full" />
							<Button href="/logout" color="purple">Logout</Button>
						</div>
					</Popover>
				</li>
			{:else}
				<li class="hover:text-gray-500">
					<a class="p-2" href="/auth/login">LOGIN</a>
				</li>
			{/if}
		</ul>
	</div>
</header>

{#if open}
	<div class="lg:hidden">
		<nav class="z-20 fixed w-[100%] p-4 flex flex-col gap-2 items-center bg-white/60" transition:fade={options}>
			{#each menus as menu}
				{#if !menu.authenticated || user}
					<li class="hover:text-gray-300">
						<a class="p-2" href={menu.href} on:click={closeMenu}>{menu.title}</a>
					</li>
				{/if}
			{/each}
		</nav>
	</div>
{/if}
