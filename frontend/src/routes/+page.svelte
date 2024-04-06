<script>
    import { dev } from "$app/environment";
    let url = location.protocol + "//" + location.host;
    if (dev) {
        url = "http://localhost:5000";
    }

    let count = 0;

    function increment() {
        count++;
    }

    let downhill = 0;
    let uphill = 0;
    let length = 0;

    let prediction = "n.a.";
    let din33466 = "n.a.";
    let sac = "n.a.";

    async function predict() {
        let result = await fetch(
            url +
                "/api/predict?" +
                new URLSearchParams({
                    downhill: downhill,
                    uphill: uphill,
                    length: length,
                }),
            {
                method: "GET",
            },
        );
        let data = await result.json();
        console.log(data);
        prediction = data.time;
        din33466 = data.din33466;
        sac = data.sac;
    }
</script>

<style>
    h1 {
        color: #333;
        text-align: center;
    }
    p, button, input, label {
        font-family: 'Arial', sans-serif;
        margin: 10px auto;
        display: block;
        text-align: center;
    }
    input[type=number], input[type=range] {
        cursor: pointer;
    }
    button {
        background-color: #008CBA;
        color: white;
        padding: 12px 20px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: 0.3s;
    }
    button:hover {
        background-color: #007B9A;
    }
    table {
        margin: auto;
        border-collapse: collapse;
    }
    td, th {
        border: 1px solid #ddd;
        text-align: left;
        padding: 8px;
    }
    tr:nth-child(even) {
        background-color: #f2f2f2;
    }
    tr:hover {
        background-color: #ddd;
    }
</style>

<h1>HikePlanner</h1>
<p>
    Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
</p>

<button on:click={increment}>
    Clicked {count} {count === 1 ? "time" : "times"}
</button>

<div class="slider-container">
    <p>
        <strong>Abwärts [m]</strong>
        <label>
            <input type="number" bind:value={downhill} min="0" max="10000" />
            <input type="range" bind:value={downhill} min="0" max="10000" />
        </label>
    </p>

    <p>
        <strong>Aufwärts [m]</strong>
        <label>
            <input type="number" bind:value={uphill} min="0" max="10000" />
            <input type="range" bind:value={uphill} min="0" max="10000" />
        </label>
    </p>

    <p>
        <strong>Distanz [m]</strong>
        <label>
            <input type="number" bind:value={length} min="0" max="30000" />
            <input type="range" bind:value={length} min="0" max="30000" />
        </label>
    </p>
</div>

<button on:click={predict}>Predict</button>

<table>
    <tr>
        <td>Dauer:</td><td>{prediction}</td>
    </tr>
    <tr>
        <td>DIN33466:</td><td>{din33466}</td>
    </tr>
    <tr>
        <td>SAC:</td><td>{sac}</td>
    </tr>
</table>
