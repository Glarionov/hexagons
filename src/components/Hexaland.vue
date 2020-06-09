<template>
    <div class="hexaland-main">
        <div v-for="(item,index) in hexagonsData" :key="index">
            <Hexagone :pos="index - 2" :hexid="index - 2"/>
        </div>
    </div>
</template>

<script>
    import Hexagone from "./Hexagone";

    export default {
        name: "Hexaland",
        components: {
            Hexagone,
        },
        data: function () {
            return {
                hexagonsData: [
                    {
                        'day': 'a'
                    },
                    {
                        'day': 'b'
                    }, {
                        'day': 'c'
                    }, {
                        'day': 'd'
                    }, {
                        'day': 'e'
                    },
                ]
            }
        }
    }
</script>

<style >


    html, body {
        height: 100%;
        margin: 0px;
    }

    body {
    }

    .hexaland-main {
        height: 100%;
        background: radial-gradient(circle at center, white -32%, #181E3E, #000000);
    }

</style>