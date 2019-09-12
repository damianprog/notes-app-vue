<template>
    <div id="app">
        <notes-list></notes-list>
    </div>
</template>

<script>
    import NotesList from './components/NotesList.vue'

    export default {
        name: 'app',
        components: {
            NotesList
        }
    }
</script>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        min-height: 100vh;
        /* #0086c3 */
        background: linear-gradient(to bottom right, #29b6f6, #29b6f6);
    }

    #app {
        font-family: Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }
</style>
