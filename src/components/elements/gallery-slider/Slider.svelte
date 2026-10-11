<script>
    import { tick } from "svelte";

    const total = 12;
    const images = Array.from({ length: total }, (_, i) => ({
        src: `/images/gallery-slider/img${i + 1}.jpg`,
        alt: `Realizacja ${i + 1}`,
    }));

    const slides = [images[total - 1], ...images, images[0]];

    let currentIndex = 1;
    let withTransition = true;
    let track;
    let zoomed = null;

    function isOnClone() {
        return currentIndex <= 0 || currentIndex >= slides.length - 1;
    }

    function next() {
        if (isOnClone()) return;
        currentIndex += 1;
    }

    function prev() {
        if (isOnClone()) return;
        currentIndex -= 1;
    }

    async function handleTransitionEnd(event) {
        if (event.target !== track) return;
        if (!isOnClone()) return;

        withTransition = false;
        currentIndex = currentIndex === 0 ? slides.length - 2 : 1;

        await tick();
        track.getBoundingClientRect();
        withTransition = true;
    }

    function openZoom(image) {
        zoomed = image;
    }

    function closeZoom() {
        zoomed = null;
    }

    function handleKeydown(event) {
        if (event.key === "Escape" && zoomed) closeZoom();
    }
</script>

<svelte:window on:keydown={handleKeydown} />

<div class="gallery__slider-inner">
    <button
        class="gallery__slider-btn gallery__slider-btn--prev"
        on:click={prev}
        aria-label="Poprzednie zdjęcie"
    >
        <svg
            viewBox="0 0 24 24"
            width="20"
            height="20"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
        >
            <path
                d="M15 18l-6-6 6-6"
                stroke-linecap="round"
                stroke-linejoin="round"
            />
        </svg>
    </button>

    <div class="gallery__slider-viewport">
        <div
            bind:this={track}
            class="gallery__slider-track"
            class:gallery__slider-track--no-transition={!withTransition}
            style="transform: translateX(-{currentIndex * 100}%)"
            on:transitionend={handleTransitionEnd}
        >
            {#each slides as image}
                <div class="gallery__slider-slide">
                    <button
                        type="button"
                        class="gallery__slider-zoom"
                        on:click={() => openZoom(image)}
                        aria-label="Powiększ zdjęcie"
                    >
                        <img
                            class="gallery__slider-img"
                            src={image.src}
                            alt={image.alt}
                            loading="lazy"
                        />
                    </button>
                </div>
            {/each}
        </div>
    </div>

    <button
        class="gallery__slider-btn gallery__slider-btn--next"
        on:click={next}
        aria-label="Następne zdjęcie"
    >
        <svg
            viewBox="0 0 24 24"
            width="20"
            height="20"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
        >
            <path
                d="M9 18l6-6-6-6"
                stroke-linecap="round"
                stroke-linejoin="round"
            />
        </svg>
    </button>
</div>

{#if zoomed}
    <button
        type="button"
        class="gallery__slider-lightbox"
        on:click={closeZoom}
        aria-label="Zamknij powiększenie"
    >
        <img
            class="gallery__slider-lightbox-img"
            src={zoomed.src}
            alt={zoomed.alt}
        />
    </button>
{/if}
