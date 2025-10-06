<script>
  export let selectedOption = "";
  export let selectedOptionGroup = "";
  export let disabled = false;

  function handleSelect(event) {
    selectedOption = event.target.value;
    selectedOptionGroup = event.target.selectedOptions[0].parentNode.label;
  }

  export let required = "false";
  let actualRequired = JSON.parse(required);

  export let label = "";

  let attachmentArray = Object();
  attachmentArray["Bone"] = [
    "ValveBiped.Bip01_Pelvis",
    "ValveBiped.Bip01_Spine",
    "ValveBiped.Bip01_Spine1",
    "ValveBiped.Bip01_Spine2",
    "ValveBiped.Bip01_Spine4",
    "ValveBiped.Bip01_Neck1",
    "ValveBiped.Bip01_Head1",
    "ValveBiped.forward",
    "ValveBiped.Bip01_R_Clavicle",
    "ValveBiped.Bip01_R_UpperArm",
    "ValveBiped.Bip01_R_Forearm",
    "ValveBiped.Bip01_R_Hand",
    "ValveBiped.Bip01_L_Clavicle",
    "ValveBiped.Bip01_L_UpperArm",
    "ValveBiped.Bip01_L_Forearm",
    "ValveBiped.Bip01_L_Hand",
    "ValveBiped.Bip01_R_Thigh",
    "ValveBiped.Bip01_R_Calf",
    "ValveBiped.Bip01_R_Foot",
    "ValveBiped.Bip01_R_Toe0",
    "ValveBiped.Bip01_L_Thigh",
    "ValveBiped.Bip01_L_Calf",
    "ValveBiped.Bip01_L_Foot",
    "ValveBiped.Bip01_L_Toe0",
    "ValveBiped.Bip01_L_Finger4",
    "ValveBiped.Bip01_L_Finger41",
    "ValveBiped.Bip01_L_Finger42",
    "ValveBiped.Bip01_L_Finger3",
    "ValveBiped.Bip01_L_Finger31",
    "ValveBiped.Bip01_L_Finger32",
    "ValveBiped.Bip01_L_Finger2",
    "ValveBiped.Bip01_L_Finger21",
    "ValveBiped.Bip01_L_Finger22",
    "ValveBiped.Bip01_L_Finger1",
    "ValveBiped.Bip01_L_Finger11",
    "ValveBiped.Bip01_L_Finger12",
    "ValveBiped.Bip01_L_Finger0",
    "ValveBiped.Bip01_L_Finger01",
    "ValveBiped.Bip01_L_Finger02",
    "ValveBiped.Bip01_R_Finger4",
    "ValveBiped.Bip01_R_Finger41",
    "ValveBiped.Bip01_R_Finger42",
    "ValveBiped.Bip01_R_Finger3",
    "ValveBiped.Bip01_R_Finger31",
    "ValveBiped.Bip01_R_Finger32",
    "ValveBiped.Bip01_R_Finger2",
    "ValveBiped.Bip01_R_Finger21",
    "ValveBiped.Bip01_R_Finger22",
    "ValveBiped.Bip01_R_Finger1",
    "ValveBiped.Bip01_R_Finger11",
    "ValveBiped.Bip01_R_Finger12",
    "ValveBiped.Bip01_R_Finger0",
    "ValveBiped.Bip01_R_Finger01",
    "ValveBiped.Bip01_R_Finger02",
    "ValveBiped.Bip01_L_Ulna",
    "ValveBiped.Bip01_L_Wrist",
    "ValveBiped.Bip01_L_Elbow",
    "ValveBiped.Bip01_L_Bicep",
    "ValveBiped.Bip01_L_Shoulder",
    "ValveBiped.Bip01_L_Trapezius",
    "ValveBiped.Bip01_R_Ulna",
    "ValveBiped.Bip01_R_Wrist",
    "ValveBiped.Bip01_R_Elbow",
    "ValveBiped.Bip01_R_Bicep",
    "ValveBiped.Bip01_R_Shoulder",
    "ValveBiped.Bip01_R_Trapezius",
  ];
  attachmentArray["Attachment"] = [
    "eyes",
    "lefteye",
    "righteye",
    "nose",
    "mouth",
    "chest",
    "hips",
    "lefthand",
    "righthand",
    "forward",
    "anim_attachment_RH",
    "anim_attachment_LH",
    "anim_attachment_head",
  ];
</script>

<label
  >{label}:
  <select id="dropdown" on:change={handleSelect} required={actualRequired} disabled={disabled}>
    <option value="">Please choose an option</option>
    {#each Object.keys(attachmentArray) as attachment}
      <optgroup label={attachment}>
        {#each attachmentArray[attachment] as attachmentOption}
          <option value={attachmentOption}>{attachmentOption}</option>
        {/each}
      </optgroup>
    {/each}
  </select>
</label>

<style>
  select {
    height: 40px;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    background-color: #2d2d2d;
    position: relative;
    color: #f5f5f5;
    min-width: 300px;
  }

  select:disabled {
    opacity: 0.6;
    pointer-events: none;
    filter: grayscale(80%);
    background-color: #1f1f1f;
    color: #aaaaaa;
    border-color: #444;
  }

  label {
    text-align: right;
  }
</style>
