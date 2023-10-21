<script lang="ts">
    import "../app.css";
    import Header from '$lib/components/Header.svelte';
    import backgroundImage from '$lib/images/ikan1.jpg';

    import { invalidate } from '$app/navigation';
    import { onMount } from 'svelte';
    import type { LayoutData, PageData } from './$types';

    export let data: LayoutData;

	$: ({ supabase, session, profile } = data);

	onMount(() => {
		const {
			data: { subscription }
		} = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});

		return () => subscription.unsubscribe();
	});

    $: user = data.session?.user;
</script>

<svelte:head>
	<title>Calm Water</title>
</svelte:head>

<main class="flex flex-col w-screen min-h-full bg-white text-black box-border">
    <Header user={profile} />

    <slot />
</main>
