<script lang="ts">
    import Sun from "lucide-svelte/icons/sun";
    import Moon from "lucide-svelte/icons/moon";

    import { toggleMode } from "mode-watcher";
    import { Button } from "$lib/components/ui/button/index.js";
    import Input from "$lib/components/ui/input/input.svelte";

    let test: string;
    let buttonmagic = false;

    function toggleButtonMagic() {
        buttonmagic = !buttonmagic;
    }

    // import VideoPlayer from "svelte-video-player";
    // import { AudioPlayer } from "svelte-mp3";
    // const poster = "https://www.server.com/poster.jpg";
    let url: string =
        "https://rr5---sn-qxaelnl7.googlevideo.com/videoplayback?expire=1724924036&ei=JOzPZvSzH5aM6dsPmtvxyAo&ip=2a03%3A4000%3A54%3A110%3A6a0b%3A2529%3A9f5c%3A4883&id=o-ALqDPJHMCqtgjfOvGJROmFeY70XncVztbBTgHs95e_kF&itag=140&source=youtube&requiressl=yes&xpc=EgVo2aDSNQ%3D%3D&bui=AQmm2ewCdsTPl_ff79RZI4HeyYtXc2ewsH0hws44_KJQSrUUFtn46PwaWNl9OmDyRFYfLZKtPa3WfSN9&spc=Mv1m9nMZzTBTXyI4aT48MsjDzRoeBWbnTh8GGaivIu9REiGfh2HMr9lCYJE_&vprv=1&svpuc=1&mime=audio%2Fmp4&ns=LuV6XWAHLgc_-O4u_wbck0cQ&rqh=1&gir=yes&clen=116566364&ratebypass=yes&dur=7202.574&lmt=1700941777122415&keepalive=yes&c=WEB&sefc=1&txp=5432434&n=TA2TwXRg-lgVDA&sparams=expire%2Cei%2Cip%2Cid%2Citag%2Csource%2Crequiressl%2Cxpc%2Cbui%2Cspc%2Cvprv%2Csvpuc%2Cmime%2Cns%2Crqh%2Cgir%2Cclen%2Cratebypass%2Cdur%2Clmt&sig=AJfQdSswRgIhAM3F5yU9xd5rnN2uftkSz4xm3_UXyrgHXxfFXvVaH4mgAiEAzFvWa3ssKb8rVvfEyjTD9yq2LL82U01AK1L9ihMLyO4%3D&pot=MnQqlJW_VpgsViqske0Y1UKRwRstD7LvV4c0aFe7hqxWL15JCOl7Xwsf3YCB8zbNRW5KWf5vp8K6d3funZ8_EOuw6QJ_O3LeDlLrTReYL12PkK7uC-qH57sGkcCqSDCraS5D-hHFGAdw_FCUt2EwCZNGy0LabQ%3D%3D&host=rr1---sn-5oxmp55u-8pxe.googlevideo.com&rm=sn-5oxmp55u-8pxe7e,sn-4g5ekz7s&rrc=79,104,80&fexp=24350457,24350516,24350517,24350557,24350561&req_id=269e7fc6c6cca3ee&ipbypass=yes&redirect_counter=3&cm2rm=sn-ci5gup-cvhk7k&cms_redirect=yes&cmsv=e&mh=Jq&mip=2401:4900:1f33:c5bc:da5e:d3ff:fe1c:cc65&mm=30&mn=sn-qxaelnl7&ms=nxu&mt=1724902355&mv=m&mvi=5&pl=54&lsparams=ipbypass,mh,mip,mm,mn,ms,mv,mvi,pl&lsig=AGtxev0wRgIhAO59YgvAklXwUC83wSB2-M-XktiwW4sWU6PGAtBZ9MHAAiEA4L15z96ZjuSpcSYI2vRLsDPlJm39umbNItkc32RZmig%3D";
</script>

<Button on:click={toggleMode} variant="outline" size="icon" class="mb-6">
    <Sun
        class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
    />
    <Moon
        class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
    />
    <span class="sr-only">Toggle theme</span>
</Button>

<Input bind:value={test} />
{test}
<!-- <Youtube id={test} />  -->

<Button on:click={toggleButtonMagic}>Click me</Button>
{#if buttonmagic}{:else}{/if}
