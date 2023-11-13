<script lang="ts">
    import fishImage from '$lib/images/ikan1.jpg';
    import { Hamburger } from 'svelte-hamburgers';

    import { Select, Label, Input } from 'flowbite-svelte';

    export let type: any;
    export let items: any;
    export let filters: any = [];
    export let onChange: any;

    const numberFormat = new Intl.NumberFormat();
</script>
<div class="flex flex-col md:flex-row min-h-full  bg-no-repeat text-white">
    <div class="w-full md:w-[25%] lg:w-[20%] h-full flex justify-center p-4">
        <div class="flex flex-col gap-1 md:gap-4 w-full">
            {#each filters as filter}
                {#if filter.type === 'select'}
                    <div class="flex flex-col gap-2 w-full">
                        <Label defaultClass="font-semibold text-white" for={filter.name}>{filter.label}</Label>
                        <Select id={filter.name} class="w-full border-black focus:ring-purple-500 focus:border-purple-500" items={filter.selections} bind:value={filter.selected} on:change={onChange}/>
                    </div>
                {:else if filter.type === 'textbox'}
                    <div class="flex flex-col gap-2 w-full">
                        <Label defaultClass="font-semibold text-white" for={filter.name}>{filter.label}</Label>
                        <Input id={filter.name} class="w-full border-black focus:ring-purple-500 focus:border-purple-500" bind:value={filter.value} on:change={onChange}/>
                    </div>
                {/if}
            {/each}
        </div>
    </div>
    <div class="w-full md:w-[75%] lg:w-[80%] flex min-[240px]:gap-4 md:gap-3 flex-wrap h-full p-4 justify-center md:justify-start align-center items-center md:content-center content-between">
        {#if items}
        {#each items as item}
            <a href="/{type}/{item.id}">
                <div class="flex flex-col w-36 min-[400px]:w-44 md:w-56 rounded-md gap-0 border-0 shadow-2xl h-full bg-purple-500/20">
                    <div class="h-44 md:h-64 overflow-hidden rounded-tl-md rounded-tr-md">
                        <img class="w-full h-[100%] md:w-full md:h-full object-cover" src="{item.image ?? fishImage}" alt="">
                    </div>
                    <div class="rounded-md px-3 py-2">
                        <h2 class="text-base whitespace-nowrap overflow-hidden text-ellipsis max-w-[100%]">{item.name}</h2>
                        <h3 class="text-sm font-bold">Rp. {numberFormat.format(item.price)}</h3>
                        <h4 class="font-light">{item.seller.username}</h4>
                    </div>
                </div>
            </a>
            {/each}
        {:else}
        <span>Loading...</span>
        {/if}
    </div>
</div>

