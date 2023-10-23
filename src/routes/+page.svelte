<script>
    import Entrybox from "../components/entrybox.svelte";
    import Dropdown from "../components/dropdown.svelte";
    import DropdownAttachments from "../components/dropdown-attachments.svelte";
    import Numgroup from "../components/numgroup.svelte";
    import { page } from "$app/stores";

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

    // Handle the button generate logic.
    function HandleSubmit(event) {
        // const form = ;
        console.log("running");
        // console.log(event.target);
        const data = new FormData(event.target);
        // const value = "test value";

        console.log(data);
        console.log(page);

        for (let field of data) {
            const [key, value] = field;
            console.log(key, value);
        }

        // This isn't how we want to handle this, but as a quick short term solution it'll do the trick.
        // baseString = "-- `${0}`\nBB.EquipableItemData[<ID>] = {{\n";

        // Save the values and download as a text file
        // const a = document.createElement("a");
        // a.href = `data:text/plain;charset=utf-8,${encodeURIComponent(value)}`;
        // // Generate unix timestamp of now
        // const now = Date.now();

        // a.download = "equipgen-" + now + ".txt";
        // a.click();
    }
</script>

<h1>Equipment Generator</h1>
<p style="margin-bottom:2px;">
    A web based version of the original EquipGen for converting values taken out
    of PAC3 into the [BB] format
</p>

<hr />

<form on:submit|preventDefault={HandleSubmit} method="none">
    <Entrybox ph="Item Name" label="Item Name" />

    <Dropdown label="Item Slot" attachmentArray={itemType} />
    <Entrybox ph="Model Path" label="Model Path" />
    <Dropdown label="Alternative Item Slot" attachmentArray={itemType} />

    <Numgroup label="Position" />
    <Numgroup label="Angles" />

    <DropdownAttachments label="Attachment Type" />

    <Entrybox ph="1" label="Model Scale" />
    <Numgroup label="Matrix Scale" />

    <!-- Force buttons on to their own line-->
    <br />
    <br />

    <button type="submit">Generate</button>
    <button type="reset">Clear</button>
</form>

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
