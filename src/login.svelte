<script lang="ts">
    import Login_Form from "./components/login-form.svelte";

    import logo from "@/assets/logo.svg";

    let error = "";

    const login = async (event) => {
        let username = event.detail.username;
        let password = event.detail.password;

        const auth = btoa(username + ":" + password);
        
        const success = await fetch("/admin/login", {
            headers: {
                Authorization: "Basic " + auth
            }
        });

        const status = await success.status;

        if (status === 401) {
            error = "Gebruikersnaam of wachtwoord is onjuist";
        } else if (status === 200) {
            try {
                if(window.location.pathname === '/admin/') {
                    setTimeout(window.location.reload.bind(window.location), 10);
                } else {
                    setTimeout(() => { window.location.href = "/admin/" }, 10);
                }
            } catch (e) {
                console.error(e);
            }
        } else {
            error = "Er is iets fout gegaan";
        }
    }
</script>

<main>
    <div class="absolute left-1/2 -translate-x-1/2 top-[5vh]">
        <img class="h-[20vh]" src={logo} alt="">
    </div>
    <div class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2">
        <Login_Form on:login={login} {error}/>
    </div>
</main>
