<template>
    <div id="app">
        <calendar/>

    </div>
</template>

<script>

    import Calendar from './components/Calendar';


    export default {
        components: {
            Calendar,
        },
        data() {
            return {}
        }
    }
</script>

<style lang="scss">
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: $font-family-text;
    }
</style>
