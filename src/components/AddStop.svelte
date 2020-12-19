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
</script>

<main>
    <div class="container">
        <h3>Here we are!</h3>
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
            <div class="form-section">
                <label for="zip">Zip</label>
                <input id="zip" class="single-line-edit" bind:value={zip}>
            </div>
        </div>
    </div>
</main>