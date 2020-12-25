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
    import { createEventDispatcher } from 'svelte';
    import { currentRouteSeqNum } from '../util/stores.js';
    import RiderListItem from './RiderListItem.svelte';
    
    const dispatch = createEventDispatcher();
    export let routeId;
    let stop = {riders: []};
    let ridersPresent = [];
    let hideAptInfo = true;

    async function getStop() {
        ridersPresent = [];
        const res = await fetch('http://localhost:8080/api/nextstop?prevStopSeqNum=' + $currentRouteSeqNum
                                + '&routeId=' + routeId);
        stop = await res.json();
    }

    function postAttendanceRecords() {
        if (ridersPresent.length > 0) {
            fetch('http://localhost:8080/api/updateriderattendance', {
                method: "POST",
                body: JSON.stringify(ridersPresent),
                headers: {"Content-type": "application/json; charset=UTF-8"}
            }) // TODO: Probably eventually oughta do something with the status of the response...
            .catch(err => console.log(err));
        }
    }

    $: if (stop.apartment != null || stop.building != null || stop.door != null) {
        hideAptInfo = false;
    }

    onMount(async () => {
        getStop();
    });

    function loadPreviousStop() {
        if ($currentRouteSeqNum <= 0) {
            alert('You are already on the first stop');
        } else {
            $currentRouteSeqNum -= 1;
            getStop();
        }
    }

    function loadNextStop() {
        postAttendanceRecords();
        if (stop.lastStop) {
            $currentRouteSeqNum = 0;
            alert('Route complete');
            dispatch('navigate', {
                destination: 'actionmenu'
            });
        } else {
            $currentRouteSeqNum += 1;
            getStop();
        }
    }

    // TODO: Write an utility array searching
    // class that implements a binary search by
    // sorting on the id value.
    function getRiderById(id) {
        let theRider = null;
        stop.riders.forEach((r, i) => {
            if (r.riderId == id) {
                theRider = r;
            }
        });
        return theRider;
    }

    function isRiderPresent(id) {
        let isPresent = false;
        for (let i = 0; i < ridersPresent.length; i++) {
            let riderId = ridersPresent[i];
            if (riderId == id) {
                isPresent = true;
            }
        }
        return isPresent;
    }

    function updateRiderStatus(isPresent, rider) {
        if (isPresent) {
            // The rider is already present, so remove them
            for (let i = 0; i < ridersPresent.length; i++) {
                if (ridersPresent[i] == rider.riderId) {
                    // This clever bit of code sets the position you want
                    // removed to the last item in the array, then pops
                    // the last element of the array off.
                    ridersPresent[i] = ridersPresent[ridersPresent.length - 1];
                    ridersPresent.pop();
                    return;
                }
            }
        } else {
            // The rider is not there, so add them
            ridersPresent.push(rider.riderId);
        }
    }

    function handleRiderClicked(event) {
        let rider = getRiderById(event.detail.id);
        if (rider != null) {
            updateRiderStatus(isRiderPresent(rider.riderId), rider);
        } else {
            // TODO: Not sure this should ever happen;
            // if a RiderListItem component forwards an
            // event with a rider id, that rider should
            // exist in this stop's list of riders.
            console.log("Rider #" + event.detail.id + " doesn't exist in our list of riders for stop #" + stop.stopId);
        }
    }

    function addStop() {
        // TODO: Need to do some stuff so we can get
        // back here when done - probably need a store for this.
        dispatch('navigate', {
            destination: 'addstop',
            callback: 'runroute'
        });
    }
</script>

<main>
    <div class="container">
        {#await stop}
            <h4>Retrieving stop info...</h4>
        {:then stop}
            <div class="toolbar">
                <h4>{stop.streetAddr}, {stop.city} {stop.zip}</h4>
                <div class="toolbar-section">
                    <button class="btn round"><i class="fas fa-pencil-alt"></i></button>
                    <button class="btn round" on:click={addStop}><i class="fas fa-plus"></i></button>
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
                    <!--
                        I must confess some measure of annoyance with
                        this implementation.  A custom component to wrap
                        a list item for the sole purpose of being able to
                        toggle a CSS class seems a bit extravagant, but I
                        don't know of another way to do it.  If there's a
                        way to replace this component with regular list item,
                        please proceed.  Do keep in mind, though, that this
                        component is likely used elsewhere.
                    -->
                    <RiderListItem rider={rider} on:clicked={handleRiderClicked}/>
                {/each}
            </ul>
        {:catch error}
            <h4 style="color: red">Error: {error}</h4>
        {/await}
        
        <div class="bottom-buttons">
            <button class="btn" on:click={loadPreviousStop}>Previous</button>
            <button class="btn" on:click={loadNextStop}>Next</button>
        </div>
    </div>
</main>