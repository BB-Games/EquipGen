<script>
  import Entrybox from "../components/entrybox.svelte";
  import Dropdown from "../components/dropdown.svelte";
  import DropdownAttachments from "../components/dropdown-attachments.svelte";
  import Numgroup from "../components/numgroup.svelte";
  import Checkbox from "../components/checkbox.svelte";

  let itemType = Object();
  itemType = [
    "ITEM_EQUIP_SLOT_HEAD",
    "ITEM_EQUIP_SLOT_FACE",
    "ITEM_EQUIP_SLOT_ARMS",
    "ITEM_EQUIP_SLOT_CHEST",
    "ITEM_EQUIP_SLOT_LEGS",
    "ITEM_EQUIP_SLOT_AURA",
    "ITEM_EQUIP_SLOT_BACK",
    "ITEM_EQUIP_SLOT_MISC",
    "ITEM_EQUIP_SLOT_MISC_2",
    "ITEM_EQUIP_SLOT_FLASHSOUND",
  ];

  let modelPassedValidation = true;

  let formData = {
    itemName: "",
    itemSlot: "",
    modelPath: "",
    altItemSlot: "",
    defaultUnusual: false,
    position: {
      x: 0,
      y: 0,
      z: 0,
    },
    angles: {
      x: 0,
      y: 0,
      z: 0,
    },
    attachmentType: "",
    attachmentTypeGroup: "",
    modelScale: 1,
    matrixScale: {
      x: 1,
      y: 1,
      z: 1,
    },
  };

  let modelValidation = true;

  // Handle the button generate logic.
  function HandleSubmit() {
    // TODO: store the values in an array as items so we can output to a singular file

    if (formData.modelPath.search("(?![/])models") == -1) {
      alert("Model path invalid! Make sure it beings with '/models'!");
      return;
    }

    let baseString = `-- ${formData.itemName}\nBB.EquipableItemData[<ID>] = {\n`;
    baseString += `\tSlot = ${formData.itemSlot},\n`;
    if (formData.altItemSlot != "") {
      baseString += `\tAltSlot = ${formData.altItemSlot},\n`;
    }
    if (formData.defaultUnusual) {
      baseString += `\tEffectGroup = 1,\n`;
    }

    baseString += `\tModels = {\n`;
    // In future, allow adding multiple 'model' entries for multi-model items.
    baseString += `\t\t{ Model = "${formData.modelPath}", ${formData.attachmentTypeGroup} = "${formData.attachmentType}", Scale = ${formData.modelScale}, AngleOffset = Angle(${formData.angles.x}, ${formData.angles.y}, ${formData.angles.z}), PosOffset = Vector(${formData.position.x}, ${formData.position.y}, ${formData.position.z})`;

    if (
      formData.matrixScale.x > 1 ||
      formData.matrixScale.y > 1 ||
      formData.matrixScale.z > 1
    ) {
      baseString += `, Matrix = {Scale = (Vector(${formData.matrixScale.x}, ${formData.matrixScale.y}, ${formData.matrixScale.z}))`;
    }

    baseString += "}},\n\t},\n}";
    console.log(baseString);

    // Save the values and download as a text file
    const phEvent = document.createElement("a");
    phEvent.href = `data:text/plain;charset=utf-8,${encodeURIComponent(
      baseString
    )}`;
    // Generate unix timestamp of now
    const now = Date.now();

    phEvent.download = `equipgen-${now}.txt`;
    phEvent.click();
  }
</script>

<h1>Equipment Generator</h1>
<p style="margin-bottom:2px;">
  A web based version of the original EquipGen for converting values taken out
  of PAC3 into the [BB] format
</p>

<hr />

<form on:submit|preventDefault={HandleSubmit} method="none">
  <Entrybox
    ph="Item Name"
    label="Item Name"
    bind:value={formData.itemName}
    required="true"
  />

  <Dropdown
    label="Item Slot"
    attachmentArray={itemType}
    bind:selectedOption={formData.itemSlot}
    required="true"
  />
  <Entrybox
    ph="Model Path"
    label="Model Path"
    bind:value={formData.modelPath}
    required="true"
    regexValidation="(?![/])models"
    bind:passesValidation={modelPassedValidation}
    validationHint="Ensure your model path starts with '/models'"
  />

  {#if modelValidation == false}
    <div>Model path invalid!</div>
  {/if}

  <Dropdown
    label="Alternative Item Slot"
    attachmentArray={itemType}
    bind:selectedOption={formData.altItemSlot}
  />

  <Numgroup
    label="Position"
    bind:xValue={formData.position.x}
    bind:yValue={formData.position.y}
    bind:zValue={formData.position.z}
  />
  <Numgroup
    label="Angles"
    bind:xValue={formData.angles.x}
    bind:yValue={formData.angles.y}
    bind:zValue={formData.angles.z}
  />

  <DropdownAttachments
    label="Attachment Type"
    bind:selectedOption={formData.attachmentType}
    bind:selectedOptionGroup={formData.attachmentTypeGroup}
    required="true"
  />

  <Entrybox
    ph="1"
    label="Model Scale"
    bind:value={formData.modelScale}
    type="number"
  />
  <Numgroup
    label="Matrix Scale"
    bind:xValue={formData.matrixScale.x}
    bind:yValue={formData.matrixScale.y}
    bind:zValue={formData.matrixScale.z}
  />

  <Checkbox
    label="Use default unusual effects?"
    bind:set={formData.defaultUnusual}
  />

  <button type="submit">Generate</button>
  <button type="reset">Clear</button>
</form>

<span id="versionLabel">v0.2</span>
<a id="watermark" href="//bbservers.co.uk">bbservers.co.uk</a>

<style>
  hr {
    border: 5px solid #f5f5f5;
    width: 100%;
    border-width: 2px;
  }

  :global(body) {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    background-color: #1c1c1c;
    color: #f5f5f5;
    justify-content: center;
    display: grid;
  }

  h1 {
    color: #f5f5f5;
  }

  p {
    color: #f5f5f5;
  }

  button {
    background-color: #2b2b2b;
    color: #f5f5f5;
    border: none;
    border-radius: 5px;
    padding: 5px;
    margin: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #f5f5f5;
    color: #2b2b2b;
  }

  a:link {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    color: #f5f5f565;
  }

  form {
    display: grid;
    grid-template-columns: max-content max-content;
    grid-gap: 5px;
  }

  #versionLabel {
    color: #f5f5f5;
    font-size: 10px;
    position: fixed;
    bottom: 0;
    left: 0;
    margin: 5px;
  }

  #watermark {
    position: fixed;
    bottom: 0;
    right: 50%;
    font-size: 10px;
    color: #f5f5f5;
    text-decoration: none;
    margin: 5px;
  }
</style>
