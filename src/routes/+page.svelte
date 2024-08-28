<script lang="ts">
    import Sun from "lucide-svelte/icons/sun";
    import Moon from "lucide-svelte/icons/moon";

    import { toggleMode } from "mode-watcher";
    import { Button } from "$lib/components/ui/button/index.js";
    import Input from "$lib/components/ui/input/input.svelte";
    import Youtube from "svelte-youtube-embed";
    let test: string;
    let buttonmagic = false;

    function toggleButtonMagic() {
        buttonmagic = !buttonmagic;
    }
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
{#if buttonmagic}
    <Youtube id={test} />
{:else}
    <!-- do something -->
{/if}
