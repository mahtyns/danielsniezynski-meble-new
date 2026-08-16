<script>
    let open = false;

    function toggleMenu() {
        open = !open;
    }
</script>

<header class="navbar">
    <div class="navbar__wrapper">
        <div class="navbar__logo">Daniel <span>Śnieżyński</span></div>

        <div class="navbar__links">
            <ul class="navbar__links__list">
                <li class="navbar__item"><a href="#gallery">Projekty</a></li>
                <li class="navbar__item"><a href="#about">O nas</a></li>
                <li class="navbar__item"><a href="#services">Oferta</a></li>
                <li class="navbar__item"><a href="#contact">Kontakt</a></li>
                <li class="navbar__button">
                    <button class="button button--main">Zadzwoń</button>
                </li>
            </ul>
        </div>

        <button
            class="navbar__menu"
            on:click={toggleMenu}
            aria-label="Otwórz menu"
            aria-expanded={open}
        >
            <img src="/images/menu.png" alt="Navigation Menu icon" />
        </button>
    </div>

    <div class="navbar__mobile-menu" class:navbar__mobile-menu--open={open}>
        <ul class="navbar__mobile-menu__list">
            <li class="navbar__mobile-menu__item">
                <a href="#gallery" on:click={toggleMenu}>Projekty</a>
            </li>
            <li class="navbar__mobile-menu__item">
                <a href="#about" on:click={toggleMenu}>O nas</a>
            </li>
            <li class="navbar__mobile-menu__item">
                <a href="#services" on:click={toggleMenu}>Oferta</a>
            </li>
            <li class="navbar__mobile-menu__item">
                <a href="#contact" on:click={toggleMenu}>Kontakt</a>
            </li>
            <li class="navbar__mobile-menu__button">
                <button
                    class="button button--main"
                    on:click={(window.location.href = "tel:+48737494150")}
                    >Zadzwoń</button
                >
            </li>
        </ul>
    </div>
</header>
