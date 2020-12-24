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
    import { currentRouteSeqNum } from '../util/stores.js';

    export let routeId;
    let streetAddr;
    let city;
    let stateAbbr;
    let zip;
    let apartment;
    let building;
    let door;

    let hideAptFields = true;

    function addNewStop() {

    }
</script>

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