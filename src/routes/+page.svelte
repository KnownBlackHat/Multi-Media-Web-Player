<script lang="ts">
    import MediaCard from "../components/MediaCard.svelte";
    import MegaPlayer from "../components/MegaPlayer.svelte";
    let input = "";
    let videoMode = false;
    let links: string[] = [];
    let full_links: string[] = [];
    let playbackRate = 1;
    let toLoad: string[] = [];
    $: links = input.split(" ");
    $: full_links = input.split(" ");
    $: toLoad = [...links.splice(0, 50)];
</script>

<svelte:head>
    <title>Multi Media Web Player</title>
</svelte:head>

<header class="p-4">
    <div class="text-4xl m-2 mb-6 text-center underline">
        Multi Media Web Player
    </div>
    <input
        type="text"
        bind:value={input}
        class="rounded bg-gray-500 p-2 w-full text-center"
        placeholder="Paste your links here separated by space"
    />
</header>
<div
    class="text-center text-ml flex items-center justify-center sticky top-0 z-10 bg-black opacity-75"
>
    Single Player Mode: <input
        type="checkbox"
        class="m-1"
        bind:checked={videoMode}
    />
    <span class="mx-4">|</span>
    Playback Rate:
    <input
        type="number"
        min="0.1"
        max="10"
        step="0.1"
        bind:value={playbackRate}
        class="w-12 text-center rounded bg-gray-500 p-1 mx-2 text-sm"
    />x
</div>
{#if input}
    {#if videoMode}
        <MegaPlayer links={full_links} {playbackRate} />
    {:else}
        <div class="mx-2 grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-8">
            {#each toLoad as link, index}
                <MediaCard {index} src={link} {playbackRate} />
            {/each}
        </div>
        {#if links.length > 0}
            <button
                class="p-2 my-4 text-center border-2 border-green-500 rounded w-full"
                on:click={() => {
                    toLoad = [...toLoad, ...links.splice(0, toLoad.length + 8)];
                }}>Load More</button
            >
        {/if}
    {/if}
{/if}

<style>
    :global(body) {
        background-color: black;
        color: white;
    }
</style>
