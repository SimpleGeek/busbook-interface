<style>
    .login-begin {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .login-end {
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        padding-top: 0%;
    }

    @media all and (max-width: 500px) {
        .login-end {
            padding-top: 3%;
        }
    }
</style>

<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let username;
    let password;

    async function login() {
        if (!username) {
            alert('Please enter a username');
        } else if (!password) {
            alert('Please enter a password');
        } else {
            let response = await fetch(`http://localhost:8080/api/users/authenticate`)
                            .then(r => r.json());
            
            if (response.authenticated) {
                dispatch('login', {
                    role: response.roleId
                });
            } else {
                alert('You are not logged in');
                // TODO: Once the login filter and service are
                // actually functional, this should provide access
                // to the reset password functionality
            }
        }
    }
</script>

<div class="container">
    <div class="login-begin">
        <h1>Log In:</h1>
        <label for="username">Username</label>
        <input id="username" class="single-line-edit" bind:value={username}>
        <label for="password">Password</label>
        <input id="password" class="single-line-edit" type="password" bind:value={password}>
    </div>

    <div class="login-end">
        <button class="btn" on:click="{login}"><i class="fas fa-arrow-alt-circle-right"></i></button>
    </div>
</div>