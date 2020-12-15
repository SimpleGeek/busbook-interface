<style>
    .bottom-buttons {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .toolbar {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .toolbar button {
        margin-right: 10%;
        margin-top: 6%;
        background-color: rgba(0, 0, 0, 0.100);
        padding: 6px 8.5px;
    }

    .toolbar button:hover {
        background-color: rgba(0, 0, 0, 0.200);
    }

    .toolbar-section {
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
    }

    ul {
        margin-bottom: 0.8%;
    }

    @media all and (max-width: 500px) {
        main {
            padding-top: 3%;
        }

        .toolbar {
            flex-direction: column;
        }

        .toolbar button {
            margin-top: 2%;
            margin-right: 1.5%;
        }

        .toolbar-section {
            order: -1;
        }
    }
</style>

<script>
    import { onMount } from 'svelte';
    
    export let routeId;
    let prevStopSeq = 0;
    let stop = {riders: []};
    let ridersPresent = [];
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

    function loadPreviousStop() {
        if (prevStopSeq <= 0) {
            alert('You are already on the first stop');
        } else {
            prevStopSeq -= 1;
            getStop();
        }
    }

    function loadNextStop() {
        prevStopSeq += 1;
        getStop();
    }
</script>

<main>
    <div class="container">
        {#await stop}
            <h3>Retrieving stop info...</h3>
        {:then stop}
            <div class="toolbar">
                <h4>{stop.streetAddr}, {stop.city} {stop.zip}</h4>
                <div class="toolbar-section">
                    <button class="btn round"><i class="fas fa-pencil-alt"></i></button>
                    <button class="btn round"><i class="fas fa-plus"></i></button>
                </div>
            </div>
            <h4 class:hide={hideAptInfo}>
                {#if stop.apartment != null}
                    {stop.apartment}
                {/if}
                {#if stop.building != null}
                    {stop.building}
                {/if}
                {#if stop.door != null}
                    {stop.door}
                {/if}
            </h4>
            <ul>
                {#each stop.riders as rider}
                    <li>{rider.fname} {rider.lname}</li>
                {/each}
            </ul>
        {:catch error}
            <h3>We had an error: {error}</h3>
        {/await}
        
        <div class="bottom-buttons">
            <button class="btn" on:click={loadPreviousStop}>Previous</button>
            <button class="btn" on:click={loadNextStop}>Next</button>
        </div>
    </div>
</main>