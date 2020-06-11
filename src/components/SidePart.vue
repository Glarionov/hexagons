<template>
    <div class="side-part-main">
        Hello I am side part
        sideSize = {{hexSideSize}}
    </div>
</template>

<script>
    export default {
        name: "SidePart",
        props: {
            'hexSideSize': Number
        }
    }
</script>

<style scoped>

    .side-part-main {
        background: blue;
        width: 100px;
        height: 100px;
        position: fixed;
        margin-left: 100px;
    }
</style>