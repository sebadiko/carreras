<script>
    // dispatch start/stop/pause/lap events following a click on the button
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    // following a click on the buttons dispatch the matching events
    function start() {
        dispatch("start");
    }
    function stop() {
        dispatch("stop");
    }
    function pause() {
        dispatch("pause");
    }

    function lap() {
        loading = true;
    }
    function lap1() {
        loading = true;
    }
    function lap2() {
        loading = true;
    }
    function lap3() {
        loading = true;
    }

    let value = "";
    let response = undefined;
    let loading = false;

    const handleInput = (event) => (value = event.target.value);

    $: if (loading == true) {
        fetch(
            `https://apicarreraejemplo.com:443/v1/public/characters?ts=1&name=${value.trim()}&apikey=8479630058c2db43f533ff3d17fb5646&hash=a8500480dfb36b9dda96150c305af8d9`
        )
            .then((res) => res.json())
            .then((json) => {
                response = json.data.results || [];
                loading = false;
                console.log(response);
            });
         
    }
   

    // consider the booleans passed down from App.svelte
    export let subscription;
    export let lapsed;
</script>
<style>
    /* display the button(s) in a row */
    div {
        display: flex;
        border: 1px solid hsl(0, 0%, 25%);
        border-radius: 20px;
    }
    div button {
        font-size: 0.9rem;
        color: hsl(0, 0%, 10%);
        font-family: inherit;
        padding: 0.5rem;
        flex-grow: 1;
        width: 0;
        border: none;
        background: none;
        border-radius: inherit;
        margin: 0; /* by default svelte applies a margin to the bottom of the button*/
        transition: background 0.2s ease-out;
    }
    /* include a border on all button but the first, to avoid a border when only one element exist */
    div button:not(:first-of-type) {
        border-left: 1px solid hsl(0, 0%, 25%);
        border-top-left-radius: 0;
        border-bottom-left-radius: 0;
    }

    div button:focus {
        outline-color: hsl(0, 0%, 25%);
    }
    div button:hover {
        background: hsl(0, 0%, 92%);
    }
</style>

<!-- depending on the value of subscription and lapsed display different controls
subscription -> Lap & pause
    otherwise, lapsed -> Reset & continue
        otherwiose -> Start
-->
<div class="controls">
    {#if subscription}
    <button on:click="{lap}">Vuelta</button>
    <button on:click="{lap1}">Vuelta</button>
    <button on:click="{lap2}">Vuelta</button>
    <button on:click="{lap3}">Vuelta</button>
    {:else if lapsed}
    <button on:click="{stop}">Reset</button>
    <button on:click="{start}">Continue</button>
    {:else}
    <button on:click="{start}">Start</button>
    {/if}
</div>

<div>
    <button on:click="{pause}">Pause</button>
</div>

