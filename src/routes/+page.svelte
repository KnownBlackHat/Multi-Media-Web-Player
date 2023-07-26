<script>
import MediaCard from "../components/MediaCard.svelte"
let input="";
let links=null;
$: input? links=input.split(" "): null
$: console.log(links)
</script>
<style>
    :global(body){
        background-color: black;
        color: white;
        }
</style>
    <header class="p-4">
        <div class="text-4xl m-2 mb-6 text-center underline">Multi Media Web Player</div>
        <input type="text" bind:value={input} class="rounded bg-gray-500 p-2 w-full text-center" placeholder="Paste your links here separated by space"/>
    </header>
        <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-8">
            {#if links}
                {#each links as link, index (index)}
                    <MediaCard src={link} />
                {/each}
            {/if}
        </div>
