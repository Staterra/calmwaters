<script>
    import img1 from '$lib/images/ikan1.jpg';
    import img2 from '$lib/images/ikan2.jpg';
    import img3 from '$lib/images/ikan3.jpg';
    import { Button } from 'flowbite-svelte';
    import { CarouselTransition } from 'flowbite-svelte'


    export const images = [
        {
            id: 0,
            name: "",
            imgurl: img1,
            attribution: "",
        },
        {
            id: 1,
            name: "",
            imgurl: img2,
            attribution: "",
        },
        {
            id: 2,
            name: "",
            imgurl: img3,
            attribution: "",
        },
    ];
</script>

<div class="flex justify-center items-center content-center w-full bg-no-repeat text-black">
    <div class="font-bold text-[6vw]">
        Welcome to Calmwaters!
    </div>
</div>
