<script>
import MediaCard from "../components/MediaCard.svelte"
let input = "";
let links = null;
let playbackRate = 1;
let toLoad = [];
$: input? links = input.split(" ") : null
$: input? toLoad = [...links.splice(0, 50)] : null
</script>
<style>
    :global(body){
        background-color: black;
        color: white;
        }
</style>

<svelte:head>
    <title>Multi Media Web Player</title> 
</svelte:head>

<header class="p-4">
    <div class="text-4xl m-2 mb-6 text-center underline">Multi Media Web Player</div>
    <input type="text" bind:value={input} class="rounded bg-gray-500 p-2 w-full text-center" placeholder="Paste your links here separated by space"/>
</header>
<div class="text-center text-2xl sticky top-0 z-10 bg-black opacity-75">
    Playback Rate: {playbackRate}x
    <input type="range" min="0.1" max="10" step="0.1" bind:value={playbackRate} class="w-full"/>
</div>
    {#if links}
        <div class="mx-2 grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-8">
            {#each toLoad as link, index}
                <MediaCard {index} src={link} {playbackRate}/>
            {/each}
        </div>
        {#if links.length > 0}
                <button class="p-2 my-4 text-center border-2 border-green-500 rounded w-full"
                on:click={()=>{
                    toLoad = [...toLoad, ...links.splice(0, toLoad.length + 8)]
                    }}>Load More</button>
        {/if}
    {/if}
