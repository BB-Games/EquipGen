<script>
  export let label = "";
  export let value = "";
  export let type = "text";
  export let ph = "unset";
  export let required = "false";

  let actualRequired = JSON.parse(required);

  const setType = (node) => {
    node.type = type;
  };
</script>

<label
  >{label}:
  <input bind:value placeholder={ph} use:setType required={actualRequired} />
</label>

<style>
  input {
    min-width: 300px;
    height: 100%;
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
