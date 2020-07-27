<template>
    <v-container fluid>
        <v-row align="center">
            <v-col class="d-flex" cols="6" sm="6">
                <v-select
                        :items="orientations"
                        v-model="orientation"
                        item-text="value"
                        item-value="value"
                        label="Orientation"
                        @change="transformLayout(orientation)"
                        dense
                        outlined
                ></v-select>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="d-flex" cols="6" sm="6">
                <v-select
                        :items="straights"
                        v-model="straight"
                        item-text="text"
                        item-value="value"
                        label="Link style"
                        @change="transformStraightLink(straight)"
                        dense
                        outlined
                ></v-select>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="d-flex" cols="6" sm="6">
                <v-switch
                        @change="toggleArrow(displayArrow)"
                        v-model="displayArrow"
                        :label="`display arrow: ${displayArrow.toString()}`"
                ></v-switch>
            </v-col>
        </v-row>
        <div class="svgContainer"></div>
    </v-container>
</template>
<style lang="scss">
    .domStyle {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;

        * {
            display: inline-block;
            font-weight: bold;
            font-size: 40px;
        }
    }
</style>
<script>
    import OrgTree from "d3-org-tree";

    const d3 = require("d3")
    const fetchDataMock = () => {
        const id1 = Math.floor(Math.random() * 10 + 10)
        return {
            "nodeId": "O-" + id1,
            "parentNodeId": "O-2",
            "width": 347,
            "height": 147,
            "borderWidth": 1,
            "borderRadius": 15,
            "borderColor": {
                "red": 15,
                "green": 140,
                "blue": 121,
                "alpha": 0.5
            },
            "backgroundColor": {
                "red": 0,
                "green": 81,
                "blue": 90,
                "alpha": 0.5
            },
            "template": "<div class=\"domStyle\"><span>Tom" + id1 + " </span></div>",
            "expanded": true,
            "added": false,
            "removed": true,
        }
    }

    export default {
        name: "Chart",
        props: ["data"],
        data() {
            return {
                chartReference: null,
                orientations: ["top-to-bottom", "bottom-to-top", "left-to-right", "right-to-left"],
                straights: [{text: "straight", value: true}, {text: "curve", value: false}],
                orientation: {value: 'right-to-left'},
                straight: {text: "curve", value: false},
                displayArrow: true,
                straightLink: false,
            };
        },
        watch: {
            data(value) {
                this.renderChart(value);
            }
        },
        mounted() {
            this.renderChart(this.data)
        },
        methods: {
            renderChart(data) {
                if (!this.chartReference) {
                    this.chartReference = new OrgTree();
                }
                this.chartReference
                    .container('.svgContainer')
                    //.svgWidth(800)
                    //.svgHeight(600)
                    .data(data)
                    //.marginLeft(-50)
                    .highlight({
                        "borderWidth": 1,
                        "borderRadius": 15,
                        "borderColor": {
                            "red": 50,
                            "green": 255,
                            "blue": 30,
                            "alpha": 1
                        },
                        "backgroundColor": {
                            "red": 20,
                            "green": 100,
                            "blue": 40,
                            "alpha": 1
                        }
                    })
                    //.current('O-2')
                    .displayArrow(this.displayArrow)
                    .straightLink(this.straightLink)
                    .collapsible(false)
                    .initialZoom(.3)
                    .onNodeClick(d => {
                        console.log(d + " node clicked")
                    })
                    .onNodeAdd(d => {
                        console.log(d + " node added")
                        this.chartReference.addNode(fetchDataMock())
                    })
                    .onNodeRemove(d => {
                        console.log(d + " node removed")
                        this.chartReference.removeNode(d)
                    })
                    .render();


                /*
                //mock trigger click
                setTimeout(() => {
                    d3.select('#O-3').dispatch('click')
                }, 2000)*/

            },

            transformLayout(direction) {
                this.chartReference.transformLayout(direction)
            },

            transformStraightLink(straight) {
                this.chartReference.transformStraightLink(straight)
            },

            toggleArrow(displayArrow) {
                this.chartReference.toggleArrow(displayArrow)
            }
        }
    }

</script>
