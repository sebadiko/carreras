<script>
    // dispatch start/stop/pause/lap events following a click on the button
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    let contador = 0;

    // following a click on the buttons dispatch the matching events
    function start() {
        dispatch("start");
    }
    function stop() {
        contador = 0;
        dispatch("stop");
    }
    function pause() {
        dispatch("pause");
    }

    function lap() {
        contador = contador + 1;
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
        border-radius: 200px;
    }
    div button {
        font-size: 1.0rem;
        color: hsl(0, 0%, 10%);
        font-family: inherit;
        padding: 0.6rem;
        flex-grow: 1;
        width: 200px;
        border: none;
        background: none;
        border-radius: inherit;
        margin: 0; /* by default svelte applies a margin to the bottom of the button*/
        transition: background 0.2s ease-out;
    }
    /* include a border on all button but the first, to avoid a border when only one element exist */
    div button:not(:first-of-type) {
        border-left: 1px solid hsl(0, 0%, 25%);
        border-top-left-radius: 5;
        border-bottom-left-radius: 5;
    }

    .controls:focus {
        outline-color: hsl(0, 0%, 0%);
    }
    .controls:hover {
        background: hsla(140, 100%, 22%, 0.616);
    }

    .team:focus {
        outline-color: hsl(0, 0%, 0%);
    }
    .team:hover{
        background-color: rgb(197, 197, 197);
    }

    .reset:focus{
        outline-color: rgb(0, 0, 0);
    }
    .reset:hover{
        background-color: rgba(155, 0, 0, 0.623);
    }

    .contador{
        border: none;
        padding-left: 50%;
    }
    .nada{
        border: 0;
        text-align: center;
    }



</style>

<!-- depending on the value of subscription and lapsed display different controls
subscription -> Lap & pause
    otherwise, lapsed -> Reset & continue
        otherwiose -> Start
-->

<div class="contador">
    <h2>
        {contador}
   </h2>     
</div>
<div class="controls">
    <button on:click="{start}">Iniciar carrera</button>
</div>

<div class="reset">
    <button on:click="{stop}">Terminar carrera</button>
</div>

<div class="nada">
    {#if subscription}
    <button class="team" on:click="{lap}">Equipo x</button>
    {:else}
    <h4> </h4>
    {/if}
</div>



