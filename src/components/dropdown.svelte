<script>
  export let selectedOption = "";
  export let optional = "false";

  function handleSelect(event) {
    selectedOption = event.target.value;
  }

  export let label = "";
  export let attachmentArray = Object();
  export let required = "false";

  let actualRequired = JSON.parse(required);
</script>

<label
  >{label}:
  <select id="dropdown" on:change={handleSelect} required={actualRequired}>
    {#if optional === "true"}
      <option value="">(Optional) Please choose an option</option>
    {:else}
      <option value="">Please choose an option</option>
    {/if}
    {#each Object.keys(attachmentArray) as attachment}
      <option value={attachmentArray[attachment]}
        >{attachmentArray[attachment]}</option
      >
    {/each}
  </select>
</label>

<style>
  select {
    min-width: 300px;
    height: 40px;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    background-color: #2d2d2d;
    position: relative;
    color: #f5f5f5;
  }

  label {
    text-align: right;
  }
</style>
