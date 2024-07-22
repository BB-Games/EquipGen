<script>
  export let label = "";
  export let value = "";
  export let type = "text";
  export let step = 1;
  export let ph = "unset";
  export let required = "false";
  export let regexValidation = "";
  export let passesValidation = true;
  export let validationHint = "";

  let actualRequired = JSON.parse(required);

  const setType = (node) => {
    node.type = type;
  };

  const validateInput = (data) => {
    if (regexValidation === "") {
      passesValidation = true;
      return;
    }

    if (data.target.value.length < 5) {
      passesValidation = true;
      return;
    }

    // Edge case to allow for a user clearing it out and trying again
    if (data.target.value == "") {
      passesValidation = true;
      return;
    }

    if (data.target.value.search(regexValidation) == -1) {
      passesValidation = false;
    } else {
      passesValidation = true;
    }
  };
</script>

<label
  >{label}:
  <input
    class={passesValidation ? "" : "invalid"}
    bind:value
    placeholder={ph}
    use:setType
    required={actualRequired}
    on:input={validateInput}
    step="{step}"
  />
</label>

{#if !passesValidation}
  <div class="invalidHover">{validationHint}</div>
{/if}

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

  .invalid {
    border: 1px solid red;
    color: red;
  }

  label {
    text-align: right;
  }

  @keyframes fadeOut {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }

  .invalidHover {
    border: 1px solid red;
    color: red;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    padding: 1rem;
    background-color: #7e080865;
    color: #ffffff;
    text-align: center;
    opacity: 1;
    transition: opacity 2s ease-in-out;
    z-index: 9999;
    animation: fadeOut 8s ease-in-out forwards;
  }
</style>
