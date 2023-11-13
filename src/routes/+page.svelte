<script>
    import tes1 from '$lib/images/1.png';
    import tes2 from '$lib/images/2.png';
    import img3 from '$lib/images/ikan3.jpg';
    import { Button } from 'flowbite-svelte';
    import { CarouselTransition } from 'flowbite-svelte';


    export const images = [
        {
            id: 0,
            name: "",
            imgurl: tes1,
            attribution: "",
        },
        {
            id: 1,
            name: "",
            imgurl: tes2,
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

<div class="justify-center items-center content-center w-full bg-no-repeat text-black">
    <div class="font-bold text-[6vw]">
        <img src="{tes2}" alt="" srcset="">
    </div>
    <div class="font-bold text-[6vw]">
        <img src="{tes1}" alt="" srcset="">
    </div>
</div>
