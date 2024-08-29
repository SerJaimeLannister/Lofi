<script lang="ts">
    import { onMount, onDestroy } from "svelte";
    import { Slider } from "$lib/components/ui/slider";

    let video_url =
        "https://rr7---sn-ci5gup-cvhk.googlevideo.com/videoplayback?expire=1724924036&ei=JOzPZvSzH5aM6dsPmtvxyAo&ip=2a03%3A4000%3A54%3A110%3A6a0b%3A2529%3A9f5c%3A4883&id=o-ALqDPJHMCqtgjfOvGJROmFeY70XncVztbBTgHs95e_kF&itag=134&aitags=133%2C134%2C135%2C136%2C137%2C160%2C242%2C243%2C244%2C247%2C248%2C278%2C394%2C395%2C396%2C397%2C398%2C399&source=youtube&requiressl=yes&xpc=EgVo2aDSNQ%3D%3D&bui=AQmm2ewCdsTPl_ff79RZI4HeyYtXc2ewsH0hws44_KJQSrUUFtn46PwaWNl9OmDyRFYfLZKtPa3WfSN9&spc=Mv1m9nMZzTBTXyI4aT48MsjDzRoeBWbnTh8GGaivIu9REiGfh2HMr9lCYJE_&vprv=1&svpuc=1&mime=video%2Fmp4&ns=LuV6XWAHLgc_-O4u_wbck0cQ&rqh=1&gir=yes&clen=69840665&ratebypass=yes&dur=7202.520&lmt=1700948394791808&keepalive=yes&c=WEB&sefc=1&txp=5432434&n=TA2TwXRg-lgVDA&sparams=expire%2Cei%2Cip%2Cid%2Caitags%2Csource%2Crequiressl%2Cxpc%2Cbui%2Cspc%2Cvprv%2Csvpuc%2Cmime%2Cns%2Crqh%2Cgir%2Cclen%2Cratebypass%2Cdur%2Clmt&sig=AJfQdSswRAIgH3GssKMZLqnY5gIE-zzUTWVcawqkNEEKtQ0o2NW5_OQCIGP6gMyZ9oxc3EuFsPpcGXJ6iLYLePBU7fVXQ0IR4nGs&pot=MnQqlJW_VpgsViqske0Y1UKRwRstD7LvV4c0aFe7hqxWL15JCOl7Xwsf3YCB8zbNRW5KWf5vp8K6d3funZ8_EOuw6QJ_O3LeDlLrTReYL12PkK7uC-qH57sGkcCqSDCraS5D-hHFGAdw_FCUt2EwCZNGy0LabQ%3D%3D&host=rr1---sn-5oxmp55u-8pxe.googlevideo.com&rm=sn-5oxmp55u-8pxe7e,sn-4g5ez67l&rrc=79,104&fexp=24350457,24350516,24350517,24350557,24350561&req_id=d5bc61a1cef1a3ee&redirect_counter=2&cms_redirect=yes&cmsv=e&ipbypass=yes&mh=Jq&mip=2401:4900:1f33:c5bc:da5e:d3ff:fe1c:cc65&mm=29&mn=sn-ci5gup-cvhk&ms=rdu&mt=1724905510&mv=m&mvi=7&pl=54&lsparams=ipbypass,mh,mip,mm,mn,ms,mv,mvi,pl&lsig=AGtxev0wRgIhAJ1mEwHrw8J1w-2B27_PWWZbp7AkqpJ_m18qiO4j7aobAiEAlbhyP4LP27IQc7CvvGWjRZ0e9FBv33eWVUZTBGBdZVM%3D";
    let audio_url =
        "https://rr4---sn-qxaeenld.googlevideo.com/videoplayback?expire=1724924036&ei=JOzPZvSzH5aM6dsPmtvxyAo&ip=2a03%3A4000%3A54%3A110%3A6a0b%3A2529%3A9f5c%3A4883&id=o-ALqDPJHMCqtgjfOvGJROmFeY70XncVztbBTgHs95e_kF&itag=140&source=youtube&requiressl=yes&xpc=EgVo2aDSNQ%3D%3D&bui=AQmm2ewCdsTPl_ff79RZI4HeyYtXc2ewsH0hws44_KJQSrUUFtn46PwaWNl9OmDyRFYfLZKtPa3WfSN9&spc=Mv1m9nMZzTBTXyI4aT48MsjDzRoeBWbnTh8GGaivIu9REiGfh2HMr9lCYJE_&vprv=1&svpuc=1&mime=audio%2Fmp4&ns=LuV6XWAHLgc_-O4u_wbck0cQ&rqh=1&gir=yes&clen=116566364&ratebypass=yes&dur=7202.574&lmt=1700941777122415&keepalive=yes&c=WEB&sefc=1&txp=5432434&n=TA2TwXRg-lgVDA&sparams=expire%2Cei%2Cip%2Cid%2Citag%2Csource%2Crequiressl%2Cxpc%2Cbui%2Cspc%2Cvprv%2Csvpuc%2Cmime%2Cns%2Crqh%2Cgir%2Cclen%2Cratebypass%2Cdur%2Clmt&sig=AJfQdSswRgIhAM3F5yU9xd5rnN2uftkSz4xm3_UXyrgHXxfFXvVaH4mgAiEAzFvWa3ssKb8rVvfEyjTD9yq2LL82U01AK1L9ihMLyO4%3D&pot=MnQqlJW_VpgsViqske0Y1UKRwRstD7LvV4c0aFe7hqxWL15JCOl7Xwsf3YCB8zbNRW5KWf5vp8K6d3funZ8_EOuw6QJ_O3LeDlLrTReYL12PkK7uC-qH57sGkcCqSDCraS5D-hHFGAdw_FCUt2EwCZNGy0LabQ%3D%3D&host=rr1---sn-5oxmp55u-8pxe.googlevideo.com&rm=sn-5oxmp55u-8pxe7e,sn-4g5ekz7s&rrc=79,104,80&fexp=24350457,24350516,24350517,24350557,24350561&req_id=4a545ca3ceea3ee&ipbypass=yes&redirect_counter=3&cm2rm=sn-ci5gup-cvhk7k&cms_redirect=yes&cmsv=e&mh=Jq&mip=2401:4900:1f33:c5bc:da5e:d3ff:fe1c:cc65&mm=30&mn=sn-qxaeenld&ms=nxu&mt=1724905715&mv=m&mvi=4&pl=54&lsparams=ipbypass,mh,mip,mm,mn,ms,mv,mvi,pl&lsig=AGtxev0wRgIhAOJa9UEUatzOa7h_0pZRBtnwMo1kLRBpvjGr_Bt4atnEAiEAvrlQNwXHmhUXi1L06wb99fvAuQNt3PfO5QP7JOl53go%3D";
    let time: number = 0;
    let timeArray: number[] = [time];
    let interval: number | undefined;
    let playing: boolean = false;
    let video: HTMLVideoElement;
    let audio: HTMLAudioElement;
    let maxTime: number = 0;
    let loading: boolean = true;

    function togglePlayback() {
        if (loading) return;

        if (playing) {
            video.pause();
            audio.pause();
        } else {
            video.play();
            audio.play();
        }
        playing = !playing;
        updateTimeInterval();
    }

    function updateTimeInterval() {
        if (interval) {
            clearInterval(interval);
        }
        if (playing) {
            interval = setInterval(() => {
                time = Math.floor(video.currentTime);
                timeArray = [time];
            }, 1000);
        }
    }

    function initializeMedia() {
        video = document.querySelector("video");
        audio = new Audio(audio_url);

        let videoLoaded = false;
        let audioLoaded = false;

        function checkLoaded() {
            if (videoLoaded && audioLoaded) {
                loading = false;
                maxTime = Math.floor(Math.max(video.duration, audio.duration));
            }
        }

        video.addEventListener("loadedmetadata", () => {
            videoLoaded = true;
            checkLoaded();
        });

        audio.addEventListener("loadedmetadata", () => {
            audioLoaded = true;
            checkLoaded();
        });

        // Ensure audio follows video if it ends
        video.addEventListener("ended", () => {
            audio.pause();
            audio.currentTime = 0;
            playing = false;
        });
    }

    function handleSliderChange() {
        if (video && audio) {
            video.currentTime = timeArray[0];
            audio.currentTime = timeArray[0];
            time = timeArray[0];
        }
    }

    function resetPlayback() {
        time = 0;
        timeArray = [0];
        if (video && audio) {
            video.currentTime = 0;
            audio.currentTime = 0;
            if (playing) {
                video.play();
                audio.play();
            }
        }
    }

    $: if (video && audio && timeArray[0] !== time) {
        handleSliderChange();
    }

    onMount(() => {
        initializeMedia();
    });

    onDestroy(() => {
        if (interval) {
            clearInterval(interval);
        }
    });
</script>

<div>
    <video bind:this={video} width="640" height="360">
        <source src={video_url} type="video/mp4" />
        Your browser does not support the video tag.
    </video>
</div>

<div>
    <Slider
        bind:value={timeArray}
        max={maxTime}
        step={1}
        on:change={handleSliderChange}
        disabled={loading}
    />
</div>

<div>
    <button on:click={togglePlayback} disabled={loading}>
        {playing ? "Pause" : "Play"}
    </button>
    <button on:click={resetPlayback} disabled={loading}> Reset </button>
</div>

{#if loading}
    <p>Loading media...</p>
{:else}
    <p>Current time: {time} / {maxTime} seconds</p>
{/if}
