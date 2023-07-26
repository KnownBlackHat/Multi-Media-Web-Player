<script>
export let src;
let downloadBtn;
</script>

<div class="rounded border-2 border-white overflow-hidden"
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
    <img class="h-48 w-full rounded" src={src} alt={src} />
{:else}
    <video class="h-48 w-full rounded" src={src} controls loop
    on:mouseenter={e => e.target.play()}>
        <track kind="captions" />
    </video>
{/if}
</div>
