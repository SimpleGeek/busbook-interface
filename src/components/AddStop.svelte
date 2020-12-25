<style>
    .hideAptFields {
        display: none;
    }

    .btn-pill {
        width: 30%;
    }

    @media all and (max-width: 500px) {
    .btn-pill {
        width: 100%;
    }
}
</style>

<script>
    import CreateRiders from './subcomponents/CreateRiders.svelte';
    import { currentRouteSeqNum } from '../util/stores.js';
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let onFirstStep = true;

    export let routeId;
    let streetAddr;
    let city;
    let stateAbbr;
    let zip;
    let apartment;
    let building;
    let door;

    let stop = {};

    let hideAptFields = true;

    function addNewStop() {
        stop = {
            stopId: 0, // This doesn't matter, won't be used for the actual insertion
            seqNum: $currentRouteSeqNum,
            streetAddr: streetAddr,
            city: city,
            stateAbbr: stateAbbr,
            zip: zip,
            apartment: apartment,
            building: building,
            door: door,
            routeId: routeId,
            isLastStop: false, // Also doesn't matter. This value isn't persisted, but is dynamically calculated on retrieval
            riders: []
        }
    }

    function addRidersToStop(event) {
        stop = event.detail.riders;

        // Post newly-created stop with riders

    }

    function postStop() {
        fetch('http://localhost:8080/api/insertstop', {
                method: "POST",
                body: JSON.stringify(stop),
                headers: {"Content-type": "application/json; charset=UTF-8"}
            }) // TODO: Probably eventually oughta do something with the status of the response...
            .catch(err => console.log(err));

        // Navigate home
        dispatch('navigate', {
            destination: 'actionmenu'
        });
    }
</script>

{#if onFirstStep}
    <main>
        <div class="container">
            <h3>Add Stop to Route {routeId}</h3>
            <div class="form">
                <div class="form-section">
                    <label for="streetaddr">Street Address</label>
                    <input id="streetaddr" class="single-line-edit" bind:value={streetAddr}>
                </div>
                <div class="form-section">
                    <label for="city">City</label>
                    <input id="city" class="single-line-edit" bind:value={city}>
                </div>
                <div class="form-section">
                    <label for="state">State</label>
                    <input id="state" class="single-line-edit" bind:value={stateAbbr}>
                </div>
                <div class="form-section">
                    <label for="zip">Zip</label>
                    <input id="zip" class="single-line-edit" bind:value={zip}>
                </div>
                <button class="btn-pill" on:click={() => {hideAptFields = !hideAptFields}}><i class="fas fa-plus"></i> Add Apartment Info</button>
                <div class:hideAptFields>
                    <div class="form-section">
                        <label for="apartment">Apartment</label>
                        <input id="apartment" class="single-line-edit" bind:value={apartment}>
                    </div>
                    <div class="form-section">
                        <label for="building">Building</label>
                        <input id="building" class="single-line-edit" bind:value={building}>
                    </div>
                    <div class="form-section">
                        <label for="door">Door</label>
                        <input id="door" class="single-line-edit" bind:value={door}>
                    </div>
                </div>
            </div>
            <div class="btn-end">
                <button class="btn" on:click="{addNewStop}">Add Stop</button>
            </div>
        </div>
    </main>
{:else}
    <CreateRiders on:complete={addRidersToStop}/>
{/if}