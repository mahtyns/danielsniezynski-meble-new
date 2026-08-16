<script>
    const total = 12;
    const images = Array.from({ length: total }, (_, i) => ({
        src: `/images/gallery-slider/img${i + 1}.jpg`,
        alt: `Realizacja ${i + 1}`,
    }));

    const slides = [images[total - 1], ...images, images[0]];

    let currentIndex = 1;
    let withTransition = true;

    function next() {
        currentIndex += 1;
    }

    function prev() {
        currentIndex -= 1;
    }

    function handleTransitionEnd() {
        if (currentIndex === slides.length - 1) {
            withTransition = false;
            currentIndex = 1;
        } else if (currentIndex === 0) {
            withTransition = false;
            currentIndex = slides.length - 2;
        }
    }

    $: if (!withTransition) {
        requestAnimationFrame(() => (withTransition = true));
    }
</script>

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
            class="gallery__slider-track"
            class:gallery__slider-track--no-transition={!withTransition}
            style="transform: translateX(-{currentIndex * 100}%)"
            on:transitionend={handleTransitionEnd}
        >
            {#each slides as image}
                <div class="gallery__slider-slide">
                    <img
                        class="gallery__slider-img"
                        src={image.src}
                        alt={image.alt}
                        loading="lazy"
                    />
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
