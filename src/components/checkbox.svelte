<script>
  export let set = false;
  export let label = "";

  // function to convert boolean to 0 or 1
</script>

<label>
  {label}:
  <input type="checkbox" bind:checked={set} />
</label>

<style>
  label {
    text-align: right;
  }

  input[type="checkbox"] {
    float: right;
  }
</style>
