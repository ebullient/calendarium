<script lang="ts">
    import { setIcon } from "obsidian";
    import { COLLAPSE, setNodeIcon } from "src/utils/icons";
    import WarningLabel from "./WarningLabel.svelte";
    import { WARNING } from "src/utils/icons";

    export let open = true;
    export let name: string;
    export let desc: string = "";
    export let warn: boolean = false;
    export let label: string | null = null;
    export let alwaysOpen = false;

    const details = (node: HTMLDetailsElement) => {
        if (open) node.setAttr("open", "open");
    };
    const handle = (node: HTMLElement) => {
        setIcon(node, COLLAPSE);
    };
</script>

<details
    class="creator calendarium-nested-settings setting-item"
    class:always-open={alwaysOpen}
    bind:open
    use:details
>
    <summary
        class="calendarium-nested-summary"
        on:keyup={(evt) => evt.preventDefault()}
    >
        <div class="setting-item setting-item-heading">
            <div class="setting-item-info">
                <div class="setting-item-name">{name}</div>
                <div class="setting-item-description">{desc}</div>
            </div>
        </div>
        <div class="right-side">
            {#if open}
                <slot name="context" class="context" />
            {/if}
            <div class="collapser">
                <div class="warning-container">
                    {#if warn}
                        <div class="x-small" use:setNodeIcon={WARNING} />
                    {/if}
                    <div class="handle" use:handle />
                </div>
                {#if warn && label}
                    <WarningLabel {label} />
                {/if}
            </div>
        </div>
    </summary>

    <div class="creator-settings-container">
        <slot />
    </div>
</details>

<style>
    .always-open {
        pointer-events: none;
    }
    .creator-settings-container {
        pointer-events: initial;
    }
    .calendarium-nested-settings {
        position: relative;
    }
    .calendarium-nested-summary {
        outline: none;
        list-style: none !important;
        list-style-type: none !important;
        min-height: 1rem;
        border-top-left-radius: 0.1rem;
        border-top-right-radius: 0.1rem;
        cursor: pointer;
        background-color: var(--creator-background-color);
        margin-right: 0;
        display: flex;
        justify-content: space-between;
    }
    .right-side {
        display: flex;
        align-items: center;
        gap: 1rem;
    }
    summary::-webkit-details-marker,
    summary::marker {
        display: none !important;
    }
    .always-open .handle {
        display: none;
    }
    .collapser {
        display: flex;
        flex-flow: column;
        justify-content: flex-start;
        align-items: flex-end;
        content: "";
    }

    .handle {
        transform: rotate(0deg);
        transition: transform 0.25s;
        display: flex;
    }

    details[open] .handle {
        transform: rotate(90deg);
    }

    .creator-settings-container {
        padding: 0.75em var(--size-4-3);
    }

    .calendarium-nested-settings {
        border-top: 0px;
    }
</style>
