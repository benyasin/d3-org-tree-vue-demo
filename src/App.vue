<template>
    <v-app>
        <v-app-bar
                app
                color="white"
        >
            <div class="d-flex align-center">
                <v-img
                        alt="Vuetify Logo"
                        class="shrink mr-2"
                        contain
                        src="https://user-images.githubusercontent.com/1866848/87675987-a08a3000-c7aa-11ea-93f4-6e164c3b0823.png"
                        transition="scale-transition"
                        width="40"
                />

                <v-toolbar-title>d3-org-tree demo</v-toolbar-title>
            </div>

        </v-app-bar>

        <v-main>
            <Chart :data="data"/>
        </v-main>
    </v-app>
</template>

<script>
    import Chart from './components/Chart';
    import data from './data.json'

    export default {
        name: 'App',

        components: {
            Chart,
        },
        created() {
            this.data = data
        },
        data: () => ({
            data: null
        }),
    };
</script>
