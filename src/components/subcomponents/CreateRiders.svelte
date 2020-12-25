<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let riders = [];

    let fname;
    let lname;
    let birthday;

    function addRider() {
        riders.push({
            riderId: 0, // Doesn't matter, will be set on backend
            fname: fname,
            lname: lname,
            birthday: birthday,
            stopId: 0 // Doesn't matter, will be set on the backend
        });
    }

    function finish() {
        dispatch('complete', {
            riders: riders
        });
    }
</script>

<main>
    <div class="container">
        <h3>Add Riders</h3>
        <div class="form">
            <div class="form-section">
                <label for="fname">First Name</label>
                <input id="fname" class="single-line-edit" bind:value={fname}>
            </div>
            <div class="form-section">
                <label for="lname">Last Name</label>
                <input id="lname" class="single-line-edit" bind:value={lname}>
            </div>
            <div class="form-section">
                <label for="birthday">Birthday</label>
                <input id="birthday" class="single-line-edit" bind:value={birthday}>
            </div>
        </div>
        <div class="btn-end">
            <div class="btn-group">
                <button>Add Another</button>
                <button>Finish</button>
            </div>
        </div>
    </div>
</main>