<script>
import MediaCard from "../components/MediaCard.svelte"
let input="";
let links=null;
let toLoad=[];
$: input? links=input.split(" "): null
$: input? toLoad = [...links.splice(0, 16)] : null
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

    {#if links}
        <div class="mx-2 grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-8">
            {#each toLoad as link}
                <MediaCard src={link} />
            {/each}
        </div>
        {#if links.length > 0}
                <button class="p-2 my-4 text-center border-2 border-green-500 rounded w-full"
                on:click={()=>{
                    toLoad = [...toLoad, ...links.splice(0, toLoad.length + 8)]
                    }}>Load More</button>
        {/if}
    {/if}
