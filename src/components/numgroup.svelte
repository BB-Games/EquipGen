<script>
    export let minSize = 0;
    export let maxSize = 10000;
    export let ph = "1";
    export let label = "";
    export let xValue = 0;
    export let yValue = 0;
    export let zValue = 0;
</script>

<label
    >{label}:
    <input
        type="number"
        min={minSize}
        max={maxSize}
        step="0.0001"
        placeholder={ph}
        bind:value={xValue}
    />
    <input
        type="number"
        min={minSize}
        max={maxSize}
        step="0.0001"
        placeholder={ph}
        bind:value={yValue}
    />
    <input
        type="number"
        min={minSize}
        max={maxSize}
        step="0.0001"
        placeholder={ph}
        bind:value={zValue}
    />
</label>

<style>
    input {
        height: 100%;
        max-width: 97.4px;
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #2d2d2d;
        position: relative;
        color: white;
    }

    label {
        text-align: right;
    }
</style>
