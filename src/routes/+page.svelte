<script>
  import Entrybox from "../components/entrybox.svelte";
  import Dropdown from "../components/dropdown.svelte";
  import DropdownAttachments from "../components/dropdown-attachments.svelte";
  import Numgroup from "../components/numgroup.svelte";
  import Checkbox from "../components/checkbox.svelte";
  import EventOptions from "../components/key-value-dropdown.svelte";

  let version = "0.4.2";
  let itemType = [
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

  let eventOptions = [
    {
      value: `\tCanEquip = function(objPlayer, objItem)
  \t\treturn BB.Events.IsChristmas() || objPlayer:IsPlatinum(), "This item can only be equipped during Christmas!"
  \tend`,
      label: "Christmas",
    },
    {
      value: `\tCanEquip = function(objPlayer, objItem)
  \t\treturn BB.Events.IsHalloween() || objPlayer:IsPlatinum(), "This item can only be equipped during Halloween!"
  \tend`,
      label: "Halloween",
    },
  ];

  let modelPassedValidation = true;

  let formData = {
    itemName: "",
    itemSlot: "",
    modelPath: "",
    altItemSlot: "",
    defaultUnusual: false,
    position: { x: 0, y: 0, z: 0 },
    angles: { x: 0, y: 0, z: 0 },
    attachmentType: "",
    attachmentTypeGroup: "",
    modelScale: 1,
    matrixScale: { x: 1, y: 1, z: 1 },
    seasonal: "",
    boneMerging: false,
  };

  let unusualCheckboxDisabled = false;
  let modelValidation = true;

  $: {
    if (
      formData.itemSlot == "ITEM_EQUIP_SLOT_HEAD" ||
      formData.itemSlot == "ITEM_EQUIP_SLOT_FACE" ||
      formData.altItemSlot == "ITEM_EQUIP_SLOT_HEAD" ||
      formData.altItemSlot == "ITEM_EQUIP_SLOT_FACE"
    ) {
      unusualCheckboxDisabled = false;
    } else {
      unusualCheckboxDisabled = true;
      formData.defaultUnusual = false;
    }
  }

  $: disableTransformFields = formData.boneMerging === true;

  function HandleSubmit() {
    if (formData.modelPath.search("(?![/])models") == -1) {
      alert("Model path invalid! Make sure it begins with '/models'!");
      return;
    }

    if (formData.modelPath.endsWith(".mdl") == false) {
      alert("Model path invalid! Make sure it ends with '.mdl'!");
      return;
    }

    let baseString = `-- ${formData.itemName}\n-- EquipGen version: ${version}\nBB.EquipableItemData[<ID>] = {\n`;
    baseString += `\tSlot = ${formData.itemSlot},\n`;
    if (formData.altItemSlot != "") {
      baseString += `\tAltSlot = ${formData.altItemSlot},\n`;
    }
    if (formData.defaultUnusual) {
      baseString += `\tEffectGroup = 1,\n`;
    }

    baseString += `\tModels = {\n`;
    baseString += `\t\t{Model = "${formData.modelPath}", `;

    if (formData.boneMerging) {
      baseString += `BoneMerge = true, Scale = ${formData.modelScale}`;
    } else {
      baseString += `${formData.attachmentTypeGroup} = "${formData.attachmentType}", Scale = ${formData.modelScale}, AngleOffset = Angle(${formData.angles.x}, ${formData.angles.y}, ${formData.angles.z}), PosOffset = Vector(${formData.position.x}, ${formData.position.y}, ${formData.position.z})`;

      const { x, y, z } = formData.matrixScale;
      const isValidScale = (val) => val > 0 && val !== 1;
      if (isValidScale(x) || isValidScale(y) || isValidScale(z)) {
        const safeX = x > 0 ? x : 1;
        const safeY = y > 0 ? y : 1;
        const safeZ = z > 0 ? z : 1;
        baseString += `, Matrix = {Scale = (Vector(${safeX}, ${safeY}, ${safeZ}))}`;
      }
    }

    baseString += "},\n\t},";

    if (formData.seasonal != "") {
      baseString += `\n\n${formData.seasonal},`;
    }

    baseString += "\n}";

    const phEvent = document.createElement("a");
    phEvent.href = `data:text/plain;charset=utf-8,${encodeURIComponent(baseString)}`;
    const now = Date.now();
    phEvent.download = `equipgen-${now}.txt`;
    phEvent.click();
  }

  function parsePACConfig(text) {
    const selfSectionMatch = text.match(
      /\["children"\]\s*=\s*\{\s*\[1\]\s*=\s*\{\s*[\s\S]*?\["self"\]\s*=\s*\{([\s\S]*?)\}\s*,\s*\},/
    );

    if (selfSectionMatch) {
      const selfContent = selfSectionMatch[1];

      const modelPathMatch = selfContent.match(/\["Model"\]\s*=\s*"([^"]+)"/);
      const positionMatch = selfContent.match(/\["Position"\]\s*=\s*Vector\(([^)]+)\)/);
      const anglesMatch = selfContent.match(/\["Angles"\]\s*=\s*Angle\(([^)]+)\)/);
      const scaleVectorMatch = selfContent.match(/\["Scale"\]\s*=\s*Vector\(([^)]+)\)/);
      const sizeMatch = selfContent.match(/\["Size"\]\s*=\s*([0-9.+-eE]+)/);

      return {
        self: {
          Model: modelPathMatch ? modelPathMatch[1] : "",
          Position: positionMatch ? parseVector(positionMatch[1]) : { x: 0, y: 0, z: 0 },
          Angles: anglesMatch ? parseVector(anglesMatch[1]) : { x: 0, y: 0, z: 0 },
          ScaleVector: scaleVectorMatch ? parseVector(scaleVectorMatch[1]) : { x: 1, y: 1, z: 1 },
          Size: sizeMatch ? parseFloat(sizeMatch[1]) : 1,
        },
      };
    } else {
      console.error("Failed to extract self section from PAC3 config.");
      return null;
    }
  }

  function parseVector(vectorString) {
    const [x, y, z] = vectorString.split(",").map((num) => parseFloat(num.trim()));
    return {
      x: truncateTo4Decimals(x || 0),
      y: truncateTo4Decimals(y || 0),
      z: truncateTo4Decimals(z || 0),
    };
  }

  function truncateTo4Decimals(num) {
    const numStr = num.toString();
    const decimalIndex = numStr.indexOf(".");
    if (decimalIndex === -1) {
      return num;
    }
    const truncatedStr = numStr.slice(0, decimalIndex + 5);
    return parseFloat(truncatedStr);
  }

  async function handleFileUpload(event) {
    const file = event.target.files[0];
    if (file) {
      const text = await file.text();
      const pacConfig = parsePACConfig(text);

      if (pacConfig && pacConfig.self) {
        formData = {
          ...formData,
          modelPath: pacConfig.self.Model || "",
          position: {
            x: pacConfig.self.Position?.x || 0,
            y: pacConfig.self.Position?.y || 0,
            z: pacConfig.self.Position?.z || 0,
          },
          angles: {
            x: pacConfig.self.Angles?.x || 0,
            y: pacConfig.self.Angles?.y || 0,
            z: pacConfig.self.Angles?.z || 0,
          },
          modelScale: truncateTo4Decimals(pacConfig.self.Size || 1),
          matrixScale: {
            x: pacConfig.self.ScaleVector?.x || 1,
            y: pacConfig.self.ScaleVector?.y || 1,
            z: pacConfig.self.ScaleVector?.z || 1,
          },
        };
      } else {
        alert("Failed to parse PAC3 config. Please ensure the file is valid.");
      }
    }
  }
</script>

<h1>Equipment Generator</h1>
<p style="margin-bottom:2px;">
  A web-based version of the original EquipGen for converting values taken out
  of PAC3 into the [BB] format.
</p>

<hr />

<details class="importer-details">
  <summary class="importer-summary">Import PAC3 File</summary>
  <div class="importer-container">
    <input
      id="pac3-file"
      type="file"
      accept=".txt,"
      on:change={handleFileUpload}
      class="importer-input"
    />
    <small class="importer-note">Upload a PAC3 export file (.txt) to auto-fill some form fields.</small>
  </div>
  <br />
</details>

<form on:submit|preventDefault={HandleSubmit} method="none">
  <Entrybox
    ph="My Incredible Item"
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
    ph="/models/my/awesome/model.mdl"
    label="Model Path"
    bind:value={formData.modelPath}
    required="true"
    regexValidation="^(?:[\\/]?)models"
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
    optional="true"
  />

  <Numgroup
    label="Position"
    bind:xValue={formData.position.x}
    bind:yValue={formData.position.y}
    bind:zValue={formData.position.z}
    disabled={disableTransformFields}
  />

  <Numgroup
    label="Angles"
    bind:xValue={formData.angles.x}
    bind:yValue={formData.angles.y}
    bind:zValue={formData.angles.z}
    disabled={disableTransformFields}
  />

  <DropdownAttachments
    label="Attachment Type"
    bind:selectedOption={formData.attachmentType}
    bind:selectedOptionGroup={formData.attachmentTypeGroup}
    required={!disableTransformFields}
    disabled={disableTransformFields}
  />

  <Entrybox
    ph="1"
    label="Model Scale"
    bind:value={formData.modelScale}
    type="number"
    step="0.001"
  />

  <Numgroup
    label="Matrix Scale"
    bind:xValue={formData.matrixScale.x}
    bind:yValue={formData.matrixScale.y}
    bind:zValue={formData.matrixScale.z}
    disabled={disableTransformFields}
  />

  <Checkbox
    label="Use default unusual effects?"
    bind:set={formData.defaultUnusual}
    disabled={unusualCheckboxDisabled}
  />

  <EventOptions
    label="Event Type"
    bind:selectedOption={formData.seasonal}
    attachmentArray={eventOptions}
    optional="true"
  />

  <Checkbox
    label="Bone-merged item?"
    bind:set={formData.boneMerging}
  />

  <button type="submit">Generate</button>
  <button type="reset">Clear</button>
</form>

<span id="versionLabel">v{version}</span>
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

  h1, p { color: #f5f5f5; }

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

  .importer-details { margin-bottom: 10px; }
  .importer-summary { cursor: pointer; font-weight: bold; color: #f5f5f5; outline: none; }
  .importer-container {
    display: grid;
    grid-template-columns: max-content max-content;
    grid-column-gap: 5px;
    align-items: center;
    margin-top: 5px;
  }

  .importer-note { grid-column: 1 / 3; font-size: 12px; opacity: 0.8; }

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
