<script lang="ts">
    export let links: string[] = [];
    export let playbackRate = 1;

    import { fade } from "svelte/transition";

    let downloadBtn: HTMLDivElement;
    let index = 0;
    let duration = 0;
    let currentTime = 0;
    $: src = links[index];

    function format(seconds: number) {
        let seconds_str = null;
        if (isNaN(seconds)) return "...";

        const minutes = Math.floor(seconds / 60);
        seconds = Math.floor(seconds % 60);
        if (seconds < 10) seconds_str = "0" + seconds;

        return `${minutes}:${seconds_str ?? seconds}`;
    }

    function handlekeydown(this: HTMLVideoElement, e: KeyboardEvent) {
        switch (e.key) {
            case "f":
                if (!document.fullscreenElement) {
                    this.play();
                    this.requestFullscreen();
                    this.controls = true;
                } else {
                    document.exitFullscreen();
                    this.controls = false;
                }
                break;
            case "n":
                next();
                break;
            case "p":
                previous();
                break;
            case " ":
                this.paused ? this.play() : this.pause();
                break;
            case "r":
                links = links.reverse();
                break;
            case "l":
                this.loop = !this.loop;
        }
    }

    function next() {
        links.length >= index && index++;
    }

    function previous() {
        index > 0 && index--;
    }
</script>

<div
    transition:fade
    class="rounded border-2 h-fit border-white overflow-hidden"
    role="button"
    tabindex={index}
    on:mouseenter={() => {
        downloadBtn.style.visibility = "visible";
    }}
    on:mouseleave={() => {
        downloadBtn.style.visibility = "hidden";
    }}
>
    <div class="relative invisible" bind:this={downloadBtn}>
        <span class="absolute top-0 right-0 p-1">
            <a
                class="bg-black text-white p-1 rounded"
                href={src}
                target="_blank"
                download
            >
                🔗
            </a>
        </span>
    </div>

    {#if src.match(/\.(jpe?g|png|gif)/)}
        <div
            class="bg-black inset-0"
            role="button"
            tabindex="0"
            on:keydown|preventDefault={handlekeydown}
            on:click={(e) => {
                ["top-0", "h-screen"].map((v) => {
                    e.currentTarget?.classList.toggle(v);
                });
                ["fixed", "z-20"].map((v) => {
                    e.currentTarget.parentElement?.classList.toggle(v);
                });
                document.body.classList.toggle("overflow-y-hidden");
            }}
        >
            <img class="rounded h-[15em] mx-auto bg-black" {src} alt={src} />
        </div>
    {:else}
        <video
            class="h-screen w-screen rounded"
            {src}
            autoplay
            preload="auto"
            playsinline
            bind:playbackRate
            bind:duration
            bind:currentTime
            on:mouseenter={(e) => e.currentTarget.focus()}
            on:keydown|preventDefault={handlekeydown}
            on:ended={next}
            on:click={(e) => {
                e.currentTarget.paused
                    ? e.currentTarget.play()
                    : e.currentTarget.pause();
            }}
        >
            <track kind="captions" />
        </video>
        <span class="flex justify-between px-2">
            <span>{format(currentTime)}</span>
            <input
                class="w-full mx-2"
                type="range"
                min="0"
                max={duration}
                step="0.01"
                bind:value={currentTime}
            />
            <span>{format(duration)}</span>
        </span>
    {/if}
    <div class="flex justify-between px-2">
        <button class="w-full p-2 bg-green-700 text-white" on:click={next}
            >Next</button
        >
        <button class="w-full p-2 bg-blue-700 text-white" on:click={previous}
            >Previous</button
        >
    </div>
</div>
