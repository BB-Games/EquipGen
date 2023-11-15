<script>
  export let set = false;
  export let label = "";
  export let disabled = false;
</script>

<label>
  {label}:
  <input type="checkbox" {disabled} bind:checked={set} />
</label>

<style>
  label {
    text-align: right;
  }

  input[type="checkbox"] {
    float: right;
  }
</style>
