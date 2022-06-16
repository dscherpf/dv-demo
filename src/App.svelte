<script lang="ts">
    import logo from './assets/svelte.png';
    import Tabs from './lib/Tabs.svelte';
    import { Theme } from 'carbon-components-svelte';
    import 'carbon-components-svelte/css/all.css';
</script>

<Theme
    render="toggle"
    toggle={{
        themes: ['g10', 'g80'],
        labelA: 'Enable dark mode',
        labelB: 'Enable dark mode',
        hideLabel: true,
        size: 'sm',
    }}
/>

<main>
    <div class="container">
        <h1>Data Vis Demo</h1>

        <Tabs />
    </div>
</main>

<style>
    :root {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
            Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }

    main {
        text-align: center;
        padding: 1em;
        margin: 0 auto;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: flex-start;
    }

    img {
        height: 16rem;
        width: 16rem;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4rem;
        font-weight: 100;
        line-height: 1.1;
        margin: 2rem auto;
        max-width: 14rem;
    }

    p {
        max-width: 14rem;
        margin: 1rem auto;
        line-height: 1.35;
    }

    @media (min-width: 480px) {
        h1 {
            max-width: none;
        }

        p {
            max-width: none;
        }
    }
    .container {
        max-width: 1400px;
        flex-grow: 1;
    }
</style>
