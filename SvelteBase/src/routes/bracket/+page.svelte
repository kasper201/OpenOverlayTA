<script lang="ts">
    let isDragging = false;
    let dragOffsetX = 0;
    let bracketX = -400;
    let bracketElement: any;

    function handleMouseDown(event: MouseEvent) {
        isDragging = true;
        const rect = bracketElement.getBoundingClientRect();
        dragOffsetX = event.clientX - rect.left;
    }

    function handleMouseMoveX(event: MouseEvent) {
        if (!isDragging) return;

        bracketX = event.clientX - dragOffsetX;

        const containerWidth = 1920;
        const bracketWidth = bracketElement.offsetWidth;

        bracketX = Math.max(-600, Math.min(bracketX, containerWidth - 200));
    }

    function handleMouseUp() {
        isDragging = false;
    }
</script>

<svelte:window on:mousemove={handleMouseMoveX} on:mouseup={handleMouseUp} />

<div class="container">    <div
    class="bracketWrapper"
    bind:this={bracketElement}
    style="transform: translateX({bracketX}px); cursor: {isDragging ? 'grabbing' : 'grab'};"
    on:mousedown={handleMouseDown}
    role="button"
    tabindex="0"
    ><iframe
        title="ChalloneBracket"
        src="https://challonge.com/bs_elites/module?theme=2&scale_to_fit=1"
        class="bracket"
        allowTransparency={true}
        frameBorder="1"
        scrolling="auto"
        style="width: 72vw; height: 100vh; border: none; pointer-events: none;"
        ></iframe>
</div>
</div>

<style>
.container {
    position: relative;
    width: 1920px;
    height: 1080px;
    background: transparent;
    overflow: hidden;
    isolation: isolate;
}

.bracketWrapper {
    position: absolute;
    user-select: none;
    z-index: 1000;
}

.bracket {
    -webkit-mask-image: linear-gradient(to right, black 50%, black 95%, transparent 99%);
    mask-image: linear-gradient(to right, black 50%, black 95%, transparent 99%);
    -webkit-mask-size: 100% 100%;
    mask-size: 100% 100%;
    -webkit-mask-repeat: no-repeat;
    mask-repeat: no-repeat;
    pointer-events: none;
}

.bracketWrapper:active {
    cursor: grabbing !important;
}
</style>
