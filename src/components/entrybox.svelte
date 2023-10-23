<script>
    export let label = "";
    export let value = "";

    export let ph = "unset";

    export const actions = {
        update: async ({ request }) => {
            // ...
            return { name: data.get("name"), success: data.get("name") !== "" };
        },
    };
</script>

<label
    >{label}:
    <input bind:value placeholder={ph} />
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
