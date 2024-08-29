<script lang="ts">
    import { onMount, onDestroy } from "svelte";
    import { Slider } from "$lib/components/ui/slider";

    let inputUrl: string = "";
    let video_url: string = "";
    let audio_url: string = "";
    let time: number = 0;
    let timeArray: number[] = [time];
    let interval: number | undefined;
    let playing: boolean = false;
    let video: HTMLVideoElement;
    let audio: HTMLAudioElement;
    let maxTime: number = 0;
    let loading: boolean = true;

    interface Format {
        init: string;
        index: string;
        bitrate: string;
        url: string;
        itag: string;
        type: string;
        clen: string;
        lmt: string;
        projectionType: string;
        container: string;
        encoding: string;
        audioQuality?: string;
        audioSampleRate?: number;
        audioChannels?: number;
        qualityLabel?: string;
        resolution?: string;
    }

    async function fetchBestFormatsUrl() {
        loading = true;
        try {
            const response = await fetch(
                `https://iv.nboeck.de/api/v1/videos/${encodeURIComponent(inputUrl)}`,
            );
            if (!response.ok) {
                throw new Error(`Error: ${response.status}`);
            }
            const data = await response.json();
            const formats: Format[] = data.adaptiveFormats;

            const bestAudioFormat = formats
                .filter((f) => f.audioQuality)
                .reduce((prev, current) => {
                    if (
                        (current.audioQuality === "AUDIO_QUALITY_MEDIUM" &&
                            prev.audioQuality === "AUDIO_QUALITY_LOW") ||
                        (current.audioQuality === prev.audioQuality &&
                            parseInt(current.bitrate) > parseInt(prev.bitrate))
                    ) {
                        return current;
                    }
                    return prev;
                });

            const bestVideoFormat = formats
                .filter((f) => f.qualityLabel)
                .reduce((prev, current) => {
                    if (
                        parseInt(current.bitrate) > parseInt(prev.bitrate) ||
                        (parseInt(current.bitrate) === parseInt(prev.bitrate) &&
                            current.resolution > prev.resolution)
                    ) {
                        return current;
                    }
                    return prev;
                });

            video_url = bestVideoFormat.url;
            audio_url = bestAudioFormat.url;
            initializeMedia();
        } catch (error) {
            console.error("Failed to fetch data:", error);
            loading = false;
        }
    }

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
        if (video) {
            video.src = video_url;
        } else {
            video = document.createElement("video");
            video.src = video_url;
        }

        if (audio) {
            audio.src = audio_url;
        } else {
            audio = new Audio(audio_url);
        }

        let videoLoaded = false;
        let audioLoaded = false;

        function checkLoaded() {
            if (videoLoaded && audioLoaded) {
                loading = false;
                maxTime = Math.floor(
                    Math.max(video.duration || 0, audio.duration || 0),
                );
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
        // We don't initialize media here anymore, as we wait for URL input
    });

    onDestroy(() => {
        if (interval) {
            clearInterval(interval);
        }
    });
</script>

<div>
    <input bind:value={inputUrl} placeholder="Enter video URL" />
    <button on:click={fetchBestFormatsUrl}>Load Video</button>
</div>

{#if video_url}
    <div>
        <video bind:this={video} width="640" height="360">
            <source src={video_url} type="video/mp4" />
            <track
                kind="captions"
                src="path_to_captions_file.vtt"
                srclang="en"
                label="English"
            />
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
{/if}
