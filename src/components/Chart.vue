<template>
    <div>
        <div class="btn-group">
            <label class="title">Orientation：</label>
            <button @click="transformLayout('top-to-bottom')">top->bottom</button>
            <button @click="transformLayout('bottom-to-top')">bottom->top</button>
            <button @click="transformLayout('left-to-right')">left->right</button>
            <button @click="transformLayout('right-to-left')">right->left</button>
        </div>
        <div class="svgContainer"></div>
    </div>
</template>
<style lang="css">
    .btn-group {
        text-align: left;
        padding: 20px;
    }

    .title {
        font-weight: bold;
        margin-right: 5%;
    }

    button {
        margin: 0 10px;
        border: 1px solid gainsboro;
        padding: 3px 10px;
        background: ghostwhite;
        border-radius: 5px;
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
                "alpha": 1
            },
            "backgroundColor": {
                "red": 0,
                "green": 81,
                "blue": 90,
                "alpha": 1
            },
            "template": "<div>\n                  <div style=\"margin-left:10px;\n                              margin-top:35px;\n                              font-size:40px;\n                              font-weight:bold;\n                         \">Jack" + id1 + "</div></div>",
            "connectorLineColor": {
                "red": 11,
                "green": 123,
                "blue": 108,
                "alpha": 1
            },
            "connectorLineWidth": 5,
            "dashArray": "",
            "expanded": true,
            "added": false,
            "removed": true,
            "directSubordinates": 9,
            "totalSubordinates": 429
        }
    }

    export default {
        name: "Chart",
        props: ["data"],
        data() {
            return {
                chartReference: null
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
                    .current('O-2')
                    .initialZoom(.4)
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
            }
        }
    }

</script>
