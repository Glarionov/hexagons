<template>
    <div class="hexagon-main" :style="customStyle">

        <div style="position: absolute; " class="hexText" :style="hexTextStyle">
       <div class="dayShower">
           17
       </div>
            <div class="monthShower">
             Июня
            </div>
            <div class="test-data">
                        pos={{hexPosition}}<br>
        hexid={{hexId}}
            </div>
       </div>
        <div style="position: relative;">

<div class="hex" :style="hexStyle">
<div class="hex-before"></div>
      <div class="hex-after"></div>
</div>




        </div>


    </div>
</template>

<script>
    export default {
        name: "Hexagone",
        data: function () {
            return {
                count: 1,
                scrollBlock: false,
                hexPosition: this.pos,
                hexId: this.hexid,
                customStyle: {
                    left: '0px',
                    right: '0px'
                },
                hexStyle: {

                },
                hexTextStyle: {

                },
                leftOffset: 0,
                windowWidth: 0,
                halfWindowWidth: 0,
                windowHeight: 0,
                hexSideSize: 0
            }
        },
        props: {
            'pos': Number,
            'hexid': Number,
        },
        created() {
            window.addEventListener('wheel', this.handleScroll);
        },
        mounted (){
            this.$nextTick(() => {
                window.addEventListener('resize', this.onResize);
            })


            this.onResize();


        },
        methods: {
            onResize() {
            this.windowHeight = window.innerHeight
            this.windowWidth = window.innerWidth
            this.halfWindowWidth = this.windowWidth / 2
            this.halfWindowHeight = this.windowHeight / 2;

            this.hexSideSize = window.innerWidth/(10 + Math.abs(this.hexPosition) * 2)

            let hexHightToSideRatio = 1.732;
            let hexHightToSideHalfRatio = 0.866;
            let halfOfDifferenceBetweenHeightAndSide = 0.366;

            let leftOffset = this.hexPosition * this.hexSideSize * 2 + this.halfWindowWidth;
            let topOffset = this.hexPosition * this.hexSideSize * -1.2 + this.halfWindowHeight;
            this.customStyle.left = leftOffset + 'px';
            this.customStyle.top = topOffset + 'px';
            this.hexStyle.width = this.hexSideSize + 'px';
            this.hexStyle.height = this.hexSideSize * hexHightToSideRatio + 'px';
            let topMargin = this.hexSideSize * hexHightToSideHalfRatio * -1;
            this.hexStyle.marginTop = topMargin + 'px';
            let leftMargin  =this.hexSideSize * halfOfDifferenceBetweenHeightAndSide * -1;
            console.log(leftMargin)
            this.hexStyle.marginLeft = this.hexSideSize * -0.5 + 'px';
            this.hexStyle.left = hexHightToSideHalfRatio + 'px;'

            this.hexTextStyle.marginLeft = (halfOfDifferenceBetweenHeightAndSide + 0.5) * -1 * this.hexSideSize + 'px';
            this.hexTextStyle.marginTop = this.hexSideSize * -0.5  + 'px';
            this.hexTextStyle.width = hexHightToSideRatio * this.hexSideSize + 'px';
            // this.hexStyle.marginLeft = this.hexSideSize * -0.70 + 'px';
            //height: 17.32em;

                console.log(this.windowWidth)
            },
            unblockScroll() {
                    console.log('unblock scroll')
                    this.scrollBlock = false;
            },
            handleScroll(event) {


                if (!this.scrollBlock) {
                    this.scrollBlock = true;
                    let direction = 1;

                    if (event.deltaY > 0) {
                        direction = -1;
                    }

                    setTimeout(this.unblockScroll, 500);
                    console.log(direction)

                    this.hexPosition += direction

                    this.onResize();
                    // Any code to be executed when the window is scrolled
                }

            }
        }
    }


</script>

<style scoped>

    .hexagon-main {
        /*background: red;*/
        /*width: 100px;*/
        /*height: 100px;*/
        /*opacity: 0.5;*/
        position: absolute;
    }

    .hexText {
        z-index: 10;
    }

    .hex:hover {
        cursor: pointer;
    }
    .hex {
        position: absolute;
        width: 10em;
        height: 17.32em;
        border-radius: 1em/.5em;
        background: white;
        transition: opacity .5s;
    }

    .hex-before, .hex-after {
        position: absolute;
        width: inherit;
        height: inherit;
        border-radius: inherit;
        background: inherit;
        content: '';

    }

    .hex-before {
        -webkit-transform: rotate(60deg);
        transform: rotate(60deg);
    }

    .hex-after {
        -webkit-transform: rotate(-60deg);
        transform: rotate(-60deg);
    }

</style>