<script>
export let src;
import { fade } from 'svelte/transition';
let downloadBtn;
</script>

<div transition:fade class="rounded border-2 h-fit border-white overflow-hidden" role="button" tabindex="0"
on:mouseenter={() => {downloadBtn.style.visibility="visible"}}
on:mouseleave={() => {downloadBtn.style.visibility="hidden"}}
>
    <div class="relative invisible" bind:this={downloadBtn}>
    <span class="absolute top-0 right-0 p-1">
        <a class="bg-black text-white p-1 rounded" href={src} target="_blank" download>
            🔗
        </a>
    </span>
    </div>
{#if src.match(/\.(jpe?g|png|gif)/)}
    <img class="h-60 rounded mx-auto" src={src} alt={src}/>
{:else}
    <video class="h-60 w-full rounded" src={src} preload="true" controls
        on:mouseenter={e => e.target.focus()}
        on:keypress={(e) => { 
            switch(e.key) {
                case "f":
                    if (!document.fullscreenElement) {
                            e.target.play()
                            e.target.requestFullscreen()
                        }
                    else {
                            e.target.pause()
                            document.exitFullscreen()
                        }
                }}}>
        <track kind="captions" />
    </video>
{/if}
</div>
