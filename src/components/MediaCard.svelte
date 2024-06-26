<script lang="ts">
    export let src;
    export let index: number;
    export let playbackRate = 1;

    import { fade } from "svelte/transition";
    import viewport from "../actions/ViewPort";

    let downloadBtn: HTMLDivElement;
    let paused = true;
    let duration = 0;
    let currentTime = 0;

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
            case " ":
                this.paused ? this.play() : this.pause();
                break;
        }
    }
</script>

<div
    transition:fade
    class="rounded border-2 h-fit border-white overflow-hidden"
    role="button"
    tabindex="0"
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
        <div class="bg-black inset-0">
            <img
                class="rounded h-[15em] mx-auto bg-black"
                use:viewport
                on:enterViewport={(e) => {
                    if (e.target.src) return;
                    e.target.src = e.target.dataset.src;
                }}
                data-src={src}
                tabindex={index}
                alt={src}
                on:click={(e) => {
                    ["top-0", "h-screen"].map((v) => {
                        e.currentTarget.classList.toggle(v);
                    });
                    ["fixed", "z-20"].map((v) => {
                        e.currentTarget.parentElement?.classList.toggle(v);
                    });
                    document.body.classList.toggle("overflow-y-hidden");
                }}
            />
        </div>
    {:else}
        <video
            async
            class="h-60 w-full rounded"
            data-src={src}
            preload="auto"
            playsinline
            loop
            use:viewport
            on:enterViewport={(e) => {
                if (e.target.src) return;
                e.target.src = e.target.dataset.src;
            }}
            on:exitViewport={(e) => {
                e.target.pause();
            }}
            tabindex={index}
            bind:playbackRate
            bind:paused
            bind:duration
            bind:currentTime
            on:mouseenter={(e) => e.currentTarget.focus()}
            on:keydown={handlekeydown}
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
</div>
