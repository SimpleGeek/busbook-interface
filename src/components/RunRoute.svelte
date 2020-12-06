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
    let stop;

    async function getStop() {
        const res = await fetch(`http://localhost:8080/api/nextstop?prevStopSeqNum=` + prevStopSeq
                                + `&routeId=` + routeId);
        return res.json();
    }

    onMount(async () => {
        stop = getStop();
    });
</script>

<main>
    <div class="container">
        {#await stop}
            <h3>Retrieving stop info...</h3>
        {:then stopJson}
            {console.log(stopJson)}
            <h3>We have the stop info!</h3>
            <ul>
                {#each stopJson.riders as rider}
                    <li>{rider.fname} {routeId.lname}</li>
                {/each}
            </ul>
        {:catch error}
            {alert(error.message)};
        {/await}
    </div>
</main>