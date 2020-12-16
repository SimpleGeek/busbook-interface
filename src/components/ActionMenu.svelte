<style>
    ul {
        list-style-type: none;
        padding-left: 1%;
    }

    li {
        margin: 0.95%;
        padding: 1.25%;
        background-color: rgba(0, 0, 0, 0.110);
    }

    li:hover {
        background-color: rgba(0, 0, 0, 0.200);
        box-shadow: 2px 2px 3px #8a8a8a;
    }

    @media all and (max-width: 500px) {
        main {
            padding-top: 3%;
        }

        ul {
            padding-left: 2.1%;
            padding-right: 2.1%;
        }

        li {
            margin: 1.75%;
            padding: 2.5%;
        }
    }
</style>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    function navigateRunRoute() {
        dispatch('navigate', {
            destination: 'runroute'
        });
    }
</script>

<main>
    <div class="container">
        <ul>
            <li on:click={navigateRunRoute}><i class="fas fa-route"></i> Run Route</li>
            <li><i class="fas fa-user-friends"></i> Go Visiting</li>
            <li><i class="fas fa-bus"></i> Manage Riders</li>
            <li><i class="fas fa-scroll"></i> Reports</li>
        </ul>    
    </div>
</main>