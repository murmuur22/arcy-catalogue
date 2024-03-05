<script lang="ts">
    import { Stretch } from 'svelte-loading-spinners';
    import { onMount } from "svelte";
    import "../app.css";

    let isReady: Boolean = false;

    onMount(() => {
        if (document.readyState === "complete") isReady = true;
    });
</script>


{#if !isReady}
    <div class="w-full h-screen flex items-center justify-center">
        <Stretch size="60" unit="px" duration="1s" color="#545454" />
    </div>
{:else}
    <slot />
{/if}

