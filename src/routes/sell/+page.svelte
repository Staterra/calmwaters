<script lang="ts">
    import { Button, Select, Modal } from 'flowbite-svelte';
    import Logocart from '~icons/material-symbols/add-shopping-cart'
    import fishImage from '$lib/images/ikan1.jpg';
    import gambarikan from '$lib/images/ikan2.jpg';
	import { onMount } from 'svelte';

    const numberFormat = new Intl.NumberFormat();

    $: showModal = false;
    $: modalTitle = 'Error';
    $: modalMessage = '';
    function openModal(title: string, message: string) {
        modalTitle = title;
        modalMessage = message;
        showModal = true;
    }

    export let data;
    let types = data.type.map((type: any) => {
        return {
            name: type.name,
            value: type.id
        }
    });

    $: showModalFish = false;
    function toggleModalFish(){
        showModalFish = !showModalFish;
    }

    $: newFish = {
        name: '',
        price: 0,
        type: '',
        dimension: 0,
        desc: '',
        image: ''
    };

    function clearFish() {
        newFish = {
            name: '',
            price: 0,
            type: '',
            dimension: 0,
            desc: '',
            image: ''
        };
    }

    async function addFish() {
        const response = await fetch('/api/fish/add', {
            method: 'POST',
            body: JSON.stringify(newFish)
        });

        const content = await response.json();

        if (response.status !== 200) {
            openModal('Error', content.message);
            return;
        }

        clearFish();
        openModal('Success', 'Successfully added a new product');
        showModalFish = false;
    }

    $: showModalUtility = false;
    function toggleModalUtility(){
        showModalUtility = !showModalUtility;
    }

    $: newUtility = {
        name: '',
        price: 0,
        desc: '',
        image: ''
    };

    function clearUtility() {
        newUtility = {
            name: '',
            price: 0,
            desc: '',
            image: ''
        };
    }

    async function addUtility() {
        const response = await fetch('/api/utility/add', {
            method: 'POST',
            body: JSON.stringify(newUtility)
        });

        const content = await response.json();

        if (response.status !== 200) {
            openModal('Error', content.message);
            return;
        }

        clearUtility();
        openModal('Success', 'Successfully created a new utility');
        showModalUtility = false;
    }

    let activeTab = 'new-order';
    async function setActiveTab(tabName: string) {
        activeTab = tabName;
        await loadItems(tabs[activeTab].status);
    }

    $: items = null;

    const tabs: any = {
        'all-product': {
            title: 'All Products',
            status: -1
        },
        'new-order': {
            title: 'New Order',
            status: 0
        },
        'ready': {
            title: 'Ready',
            status: 2
        },
        'delivering': {
            title: 'Delivering',
            status: 3
        },
        'delivered': {
            title: 'Delivered',
            status: 4
        }
    };

    async function removeProduct(item: any) {
        const response = await fetch('/api/product/remove', {
            method: 'POST',
            body: JSON.stringify({
                id: item.id,
                type: item.type
            })
        });

        if (response.status !== 200) {
            openModal('Error', 'Failed to load products');
            return;
        }

        await loadItems(-1);
    }

    async function setStatus(item: any, status: number) {
        const content: any = {
            status
        };

        if (item.type === 'fish') {
            content.fish_id = item.id;
        } else {
            content.utility_id = item.id;
        }

        const response = await fetch('/api/order/status', {
            method: 'POST',
            body: JSON.stringify(content)
        });

        if (response.status !== 200) {
            openModal('Error', 'Failed to do action');
            return;
        }

        openModal('Success', 'Action success');
        await loadItems(tabs[activeTab].status);
    }

    async function loadItems(status: number) {
        items = null;

        let response = null;

        if (status === -1) {
            response = await fetch('/api/product/list');
        } else {
            response = await fetch('/api/order/list/seller', {
                method: 'POST',
                body: JSON.stringify({
                    status
                })
            });
        }


        if (response.status !== 200) {
            openModal('Error', `Can't retrieve order data`);
            return;
        }

        const content = await response.json();
        items = content.data;
    }

    onMount(async () => {
        await loadItems(0);
    });
</script>

<div class="flex md:flex-row flex-col min-h-full bg-no-repeat text-white">
    <div class="w-full md:w-[20%] h-full flex justify-center p-4">
        <div class="flex flex-row md:flex-col justify-between gap-4 w-full">
            <a href="#all-product" on:click={() => setActiveTab('all-product')} class="w-[50%] md:w-auto md:justify-start justify-center items-center flex bg-[#724C9D] rounded-xl p-2.5 hover:bg-purple-600">
                <Logocart class="text-2xl"></Logocart>
                <h1 class="text-sm lg:text-lg">All product</h1>
            </a>
            <a href="#new-order" on:click={() => setActiveTab('new-order')} class="w-[50%] md:w-auto md:justify-start justify-center items-center flex bg-[#724C9D] rounded-xl p-2.5 hover:bg-purple-600">
                <Logocart class="text-2xl"></Logocart>
                <h1 class="text-sm lg:text-lg">New Order</h1>
            </a>
        </div>
    </div>
    <div class="w-full md:w-[80%] flex gap-3 flex-col h-full p-4 justify-start align">
        <div class="flex place-content-between items-center">
            <h1 class="text-3xl">{tabs[activeTab].title}:</h1>
            <div>
                <Button class="w-auto" on:click={toggleModalFish} color="purple">Add Product</Button>
                {#if showModalFish}
                    <div class="overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex text-white">
                    <div class="relative w-full my-6 mx-auto max-w-3xl">
                        <div class="border-0 rounded-lg shadow-lg relative flex flex-col w-full bg-gray-800 outline-none focus:outline-none">
                        <div class="flex items-start justify-between p-5 border-b border-solid border-blueGray-200 rounded-t">
                            <h3 class="text-3xl font-semibold">
                            Add Your Product Here
                            </h3>
                            <button class="p-1 ml-auto bg-transparent border-0 text-white float-right text-3xl leading-none font-semibold outline-none focus:outline-none" on:click={toggleModalFish}>
                            <span class="bg-transparent text-white h-6 w-6 text-2xl block outline-none focus:outline-none">
                                x
                            </span>
                            </button>
                        </div>
                        <div class="relative p-6 flex-auto w-full">
                            <form>
                                <div class="my-2 text-lg leading-relaxed">
                                    <label class="block text-sm font-bold mb-2" for="fishname">
                                        Product Name
                                    </label>
                                    <input bind:value={newFish.name} class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="fishname" type="text" placeholder="Product Name">
                                </div>
                                <div class="mb-4">
                                    <label class="block  text-sm font-bold mb-2" for="fishprice">
                                        Product Price
                                    </label>
                                    <input bind:value={newFish.price} class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="fishprice" type="number" placeholder="Product Price">
                                </div>
                                <div class="mb-4">
                                    <label class="block text-sm font-bold mb-2" for="fishtype">
                                        Weight
                                    </label>
                                    <input bind:value={newFish.dimension} class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="fishdimension" type="number" placeholder="Product Dimension">
                                </div>
                                <div class="mb-4">
                                    <label class="block text-sm font-bold mb-2" for="fishtype">
                                        Product Type
                                    </label>
                                    <Select bind:value={newFish.type} items={types} class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="fishtype" />
                                </div>
                                <div class="mb-4">
                                    <label class="block text-sm font-bold mb-2" for="fishdescription">
                                        Product Description
                                    </label>
                                    <textarea bind:value={newFish.desc} class="shadow resize-none text-black appearance-none border rounded w-full py-2 px-3 leading-tight focus:outline-none focus:shadow-outline" id="fishdescription" rows="3" placeholder="Product Description"></textarea>
                                </div>
                                <div class="my-2 text-lg leading-relaxed">
                                    <label class="block text-sm font-bold mb-2" for="fishname">
                                        Product Image URL
                                    </label>
                                    <input bind:value={newFish.image} class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="fishname" type="text" placeholder="Product Image">
                                </div>
                            </form>
                        </div>
                        <div class="flex items-center justify-end p-6 border-t border-solid border-blueGray-200 rounded-b">
                            <button class="text-purple-500 background-transparent font-bold uppercase px-6 py-2 text-sm outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150" type="button" on:click={toggleModalFish}>
                            Close
                            </button>
                            <button class="bg-purple-500 text-white active:bg-emerald-600 font-bold uppercase text-sm px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150" type="button" on:click={addFish}>
                            Create
                            </button>
                        </div>
                        </div>
                    </div>
                    </div>
                    <div class="opacity-25 fixed inset-0 z-40 bg-black"></div>
                {/if}

            </div>
        </div>

        <div class="flex flex-col w-full gap-4">
            {#if items}
                {#each items as item}
                    <div class="flex md:flex-row flex-col w-[100%] pb-2 gap-2">
                        <div class="h-fit md:h-40 w-full md:w-72 overflow-hidden rounded-md">
                            <img class="object-cover w-full h-full" src="{item.image ?? fishImage}" alt="">
                        </div>
                        <div class="justify-between flex flex-col w-full">
                            <div>
                                <h1 class="font-bold text-xl md:text-3xl">{item.name}</h1>
                                <h1 class="md:text-2xl text-xl">Rp. {numberFormat.format(item.price)}</h1>
                            </div>
                            {#if activeTab === 'new-order'}
                                <div class="md:flex-none flex justify-end gap-2">
                                    <Button on:click={() => setStatus(item, 2)} class="self-end w-24" color="purple">Accept</Button>
                                    <Button on:click={() => setStatus(item, 1)} class="self-end w-24" color="purple">Decline</Button>
                                </div>
                            {/if}

                            {#if activeTab === 'all-product' && item.sold === false}
                                <Button on:click={() => removeProduct(item)} class="self-end w-24" color="purple">Remove</Button>
                            {/if}
                        </div>
                    </div>
                {/each}
            {:else}
                <h1>Loading...</h1>
            {/if}
        </div>
    </div>
</div>

<Modal title={modalTitle} bind:open={showModal} autoclose outsideclose>
    <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
      {modalMessage}
    </p>
    <svelte:fragment slot='footer'>
        <Button>Close</Button>
    </svelte:fragment>
</Modal>