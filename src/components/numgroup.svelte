<script>
  export let minSize = -10000;
  export let maxSize = 10000;
  export let ph = "1";
  export let label = "";
  export let xValue = 0;
  export let yValue = 0;
  export let zValue = 0;
  export let disabled = false;
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
    disabled={disabled}
  />
  <input
    type="number"
    min={minSize}
    max={maxSize}
    step="0.0001"
    placeholder={ph}
    bind:value={yValue}
    disabled={disabled}
  />
  <input
    type="number"
    min={minSize}
    max={maxSize}
    step="0.0001"
    placeholder={ph}
    bind:value={zValue}
    disabled={disabled}
  />
</label>

<style>
  input {
    height: 100%;
    max-width: 96.7px;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    background-color: #2d2d2d;
    position: relative;
    color: white;
  }

  input:disabled {
    opacity: 0.6;
    pointer-events: none;
    filter: grayscale(80%);
    background-color: #1f1f1f; /* slightly darker to match entrybox feel */
    color: #aaaaaa; /* dimmed text */
    border-color: #444;
  }

  label {
    text-align: right;
  }
</style>
