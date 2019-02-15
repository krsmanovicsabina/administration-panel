<template>
    <div id="app">
        <calendar/>

    </div>
</template>

<script>

    import calendar from './components/calendar';


    export default {
        name: 'app',
        components: {
            calendar,

        },
        data() {
            return {}
        }
    }
</script>

<style>

    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family:Helvetica, sans-serif;
    }

</style>
