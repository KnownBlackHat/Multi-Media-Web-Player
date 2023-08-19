<script>
export let src;
export let index;
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
    <div class="bg-black inset-0"> 

    <img defer preload="auto" class="rounded  h-[15em] mx-auto bg-black"
    media-id={index} src={src} alt={src} on:click={(e) => {
        ["top-0","h-screen"].map(v=>{
                e.target.classList.toggle(v);
                });
        ["fixed", "z-20"].map(v=>{
                e.target.parentElement.classList.toggle(v);
                })
        document.body.classList.toggle("overflow-y-hidden");
    }}/>
    </div>
{:else}
    <video defer class="h-60 w-full rounded" src={src} media-id={index} preload="auto" controls
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
