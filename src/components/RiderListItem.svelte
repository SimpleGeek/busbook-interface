<style>
    .selected {
        background-color: dodgerblue;
    }
</style>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let rider;
    let selected = false;

    // This is a hack to account for the fact
    // that the same component will render different
    // riders.  When a new rider is loaded into this
    // component, we want to make sure it is unselected,
    // so we reactively assign the selected value to false,
    // based on the negation of the rider id.  This works because
    // numbers are evaluated as truthy in JavaScript, so negating
    // the numeric rider id will always give us false.
    $: selected = !(rider.riderId);

    function handleClick() {
        selected = !selected;

        dispatch('clicked', {
                    id: rider.riderId
                });
    }
</script>

<li class:selected on:click={handleClick}>{rider.fname} {rider.lname}</li>