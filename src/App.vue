<template>
    <div id="app">
        <Chart :data="data"/>
    </div>
</template>

<script>
    import Chart from './components/Chart.vue'
    import data from './data.json'

    export default {
        name: 'App',
        components: {
            Chart
        },
        data() {
            return {
                data: null
            };
        },
        created() {
            this.data = data
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
