<style>
    @media all and (max-width: 500px) {
        main {
            padding-top: 3%;
        }
    }
</style>

<script>
    import { onMount } from 'svelte';
    
    export let routeId;
    let prevStopSeq = 0;
    let stop = {riders: []};
    let hideAptInfo = true;

    async function getStop() {
        const res = await fetch(`http://localhost:8080/api/nextstop?prevStopSeqNum=` + prevStopSeq
                                + `&routeId=` + routeId);
        stop = await res.json();
    }

    $: if (stop.apartment != null || stop.building != null || stop.door != null) {
        hideAptInfo = false;
    }

    onMount(async () => {
        getStop();
    });
</script>

<main>
    <div class="container">
        {#await stop}
            <h3>Retrieving stop info...</h3>
        {:then stop}
            <h3>{stop.streetAddr}, {stop.city} {stop.zip}</h3>
            <h3 class:hide={hideAptInfo}>
                {#if stop.apartment != null}
                    Apt: {stop.apartment}
                {/if}
                {#if stop.building != null}
                    Bldg: {stop.building}
                {/if}
                {#if stop.door != null}
                    Door: {stop.door}
                {/if}
            </h3>
            <ul>
                {#each stop.riders as rider}
                    <li>{rider.fname} {rider.lname}</li>
                {/each}
            </ul>
        {:catch error}
            <h3>We had an error: {error}</h3>
        {/await}
    </div>
</main>