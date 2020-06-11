<template>
    <div class="test-main">
        Hello I am test
         <div ref="box" class="box"></div>
        <div ref="box2" class="box2"></div>
    </div>
</template>

<script>
    import { TimelineLite } from 'gsap'


    export default {
        name: "Test",
        mounted() {
            const {box, box2} = this.$refs
            const timeline = new TimelineLite()

            timeline.to(box, 1, {x: 200, rotation: 90})
            timeline.to(box2, 1, {x: 200, rotation: 90})
        }
    }
</script>

<style scoped>

    .test-main {
        width: 100px;
        height: 100px;
        background: red;
    }

    .box {
        height: 60px;
        width: 60px;
        background: blue;
    }

        .box2 {
        height: 60px;
        width: 60px;
        background: green;
            margin-top: 100px;
    }


</style>