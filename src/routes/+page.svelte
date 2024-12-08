<script lang="ts">
    import {Button} from "$lib/components/ui/button/index";

    let jsonInput = '';
    let isValid = true;
    let errorMessage = '';
    let prettyJson = '';

    function validateJson() {
        try {
            if (!jsonInput.trim()) {
                isValid = true;
                errorMessage = '';
                prettyJson = '';
                return;
            }

            const parsed = JSON.parse(jsonInput);
            prettyJson = JSON.stringify(parsed, null, 2);
            isValid = true;
            errorMessage = '';
        } catch (error: any) {
            isValid = false;
            errorMessage = error?.message;
            prettyJson = '';
        }
    }

    function handleInput() {
        validateJson();
    }

    function handlePrettify() {
        if (!jsonInput.trim()) {
            return;
        }

        try {
            // First validate the JSON
            const parsed = JSON.parse(jsonInput);
            // Update the input with the prettified version
            jsonInput = JSON.stringify(parsed, null, 2);
            // Update the pretty display
            prettyJson = jsonInput;
            isValid = true;
            errorMessage = '';
        } catch (error: any) {
            isValid = false;
            errorMessage = error?.message;
            prettyJson = '';
        }
    }
</script>

<div class="container mx-auto p-6">
    <h2 class="text-2xl font-bold mb-4">JSON Validator</h2>
    <textarea
            bind:value={jsonInput}
            on:input={handleInput}
            placeholder="Paste your JSON here..."
            rows="10"
            class="w-full p-3 font-mono border border-gray-300 rounded-md mb-3 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
    ></textarea>

    <div class="flex gap-2 mb-3">
        <Button on:click={handlePrettify} disabled={!jsonInput.trim() || !isValid}>Prettify</Button>
    </div>

    {#if jsonInput.trim()}
        <div class={`p-3 mb-3 rounded-md ${isValid ? 'bg-green-50 text-green-700' : 'bg-red-50 text-red-700'}`}>
            {#if isValid}
        <span class="flex items-center gap-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
          </svg>
          Valid JSON
        </span>
            {:else}
        <span class="flex items-center gap-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
          Invalid JSON: {errorMessage}
        </span>
            {/if}
        </div>
    {/if}

    {#if prettyJson}
        <div class="mt-6">
            <h3 class="text-lg font-semibold mb-2">Formatted JSON:</h3>
            <pre class="bg-gray-50 p-4 rounded-md text-sm">{prettyJson}</pre>
        </div>
    {/if}
</div>