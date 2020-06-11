<template>
    <div class="hexagon-main" :style="customStyle" ref="hexagonMain">

        <SidePart v-if="false"/>
        <div style="position: absolute; " class="hexText" :style="hexTextStyle" ref="hexTextStyle">

           <div v-if="hexPosition">
            <div class="dayShower">
           {{eventData.day}}
       </div>
            <div class="monthShower">
             {{eventData.month}}
            </div>
           </div>

            <div v-else>
            <div class="stadium-name">
          {{eventData.stadium}}
       </div>
            <div class="day-monthShower">
                 {{eventData.day}} {{eventData.month}}
            </div>
                <div class="event-time">
                    {{eventData.time}}
                </div>
                <div class="buy-ticket-button">
                    Купить билет
                </div>
            </div>




            <div class="test-data" v-if="false">
                        pos={{hexPosition}}<br>
        hexid={{hexId}}<br>
                hexSideSize={{hexSideSize}}<br>
                ptz={{ptz}}
            </div>
       </div>
        <div style="position: relative; width: 100%; height: 100%">

<div class="hex" :style="hexStyle" ref="hexref">
<div class="hex-before"></div>
      <div class="hex-after"></div>
</div>




        </div>


    </div>
</template>

<script>
    import { TimelineLite } from 'gsap'
    import SidePart from "./SidePart";

    export default {

        components: {
            SidePart
        },
        name: "Hexagone",
        data: function () {
            return {
                count: 1,
                scrollBlock: false,
                hexPosition: this.pos,
                hexId: this.hexid,
                ptz: 0,
                customStyle: {
                    left: '0px',
                    right: '0px'
                },
                hexStyle: {
                },
                hexTextStyle: {
                },
                rightTeamStyle: {

                },
                leftOffset: 0,
                windowWidth: 0,
                halfWindowWidth: 0,
                windowHeight: 0,
                hexSideSize: 0
            }
        },
        props: {
            pos: Number,
            hexid: Number,
            eventData: Object,
            hexParamsByPosition: Object,
            hexParamsByAbsPosition: Array
        },
        created() {
            window.addEventListener('wheel', this.handleScroll);
        },
        mounted (){
            this.$nextTick(() => {
                window.addEventListener('resize', this.handleWindowSizeChange);
            })


            // this.changeHexagones();
            // setTimeout(this.showHexagone, 200);



        },

        methods: {
            showHexagone() {
                /*gl_snippet_begin*/console.log('DDDDDDDDDDBBBBBBBBBBBB');/*gl_snippet_end*/
                const timeline = new TimelineLite()
                const {hexagonMain} = this.$refs
                timeline.to(hexagonMain, 0.2, {opacity: 1, delay: 0.2});
                this.customStyle.opacity = 1;
            },
            handleWindowSizeChange(){
                this.changeHexagones();
                this.rightTeamStyle.marginLeft = '200px';
            },
            roll3() {
            },
            changeHexagones(animate = true) {

                const {hexagonMain} = this.$refs

                const timeline = new TimelineLite()
                let absPosition = Math.abs(this.hexPosition);

                let paramsByPosition = this.hexParamsByPosition[absPosition];
                let paramsByAbsPosition = this.hexParamsByAbsPosition[this.hexPosition];

                let paramsListByPosition = this.hexParamsByPosition;
                let paramsListByAbsPosition = this.hexParamsByAbsPosition;

                paramsByPosition = paramsListByPosition[this.hexPosition];
                paramsByAbsPosition = paramsListByAbsPosition[absPosition];

                let animationVars = {};

                if (absPosition <= 2) {
                    animationVars = {
                        width: paramsByAbsPosition.sideLength,
                        height: paramsByAbsPosition.height,
                        left: paramsByPosition.mainLeftOffset,
                        top: paramsByPosition.mainTopOffset,
                        opacity: 1

                    };

                    this.hexTextStyle.marginLeft = paramsByAbsPosition.textMarginLeft + 'px';
                    this.hexTextStyle.width = paramsByAbsPosition.height + 'px';
                } else {
                    animationVars['opacity'] = 0;
                }
                if (animate) {
                    timeline.to(hexagonMain, 0.4, animationVars);
                } else {
                    timeline.to(hexagonMain, 0.1, animationVars);
                }
                this.hexTextStyle.top = '50%';



                ///////////////
                // this.windowHeight = window.innerHeight
                // this.windowWidth = window.innerWidth
                // this.halfWindowWidth = this.windowWidth / 2
                // this.halfWindowHeight = this.windowHeight / 2;
                //
                // let absPosition = Math.abs(this.hexPosition);
                //
                // let widthLimit = 1200;
                // let currentWidthWithLimit = this.windowWidth;
                // if (currentWidthWithLimit > widthLimit) {
                //     currentWidthWithLimit = widthLimit;
                // }
                // this.centralHexSideSize = currentWidthWithLimit / (8.5);
                //
                //
                // let hexHightToSideRatio = 1.732;
                // let hexHightToSideHalfRatio = 0.866;
                // // let halfOfDifferenceBetweenHeightAndSide = 0.366;
                // let sizeProportionsToZero = [1, 0.6, 0.4];
                //
                // let marginProportions = [0, 0.9, 1.5];
                // // this.ptz = sizeProportionsToZero[absPosition];
                // this.hexSideSize = this.centralHexSideSize * sizeProportionsToZero[absPosition];
                // let hexHeight = this.hexSideSize * hexHightToSideRatio;
                //
                //
                // if (absPosition > 2) {
                //     this.customStyle.opacity = 0;
                // } else {
                //     this.customStyle.opacity = 1;
                // }
                //
                //
                // let leftOffset = this.halfWindowWidth;
                // let topOffset = this.halfWindowHeight;
                //
                // const timeline = new TimelineLite()
                //
                //
                // if (absPosition) {
                //     let coef = this.hexPosition / absPosition;
                //
                //     this.ptz = marginProportions[absPosition];
                //     let marginBase = marginProportions[absPosition] * this.centralHexSideSize;
                //
                //     leftOffset += marginBase * hexHightToSideRatio * coef;
                //     topOffset += marginBase * coef * -1;
                // }
                //
                // let topMargin = this.hexSideSize * hexHightToSideHalfRatio * -1;
                // let hexMarginLeft = this.hexSideSize * -0.5;
                //
                // leftOffset += hexMarginLeft;
                // topOffset += topMargin;
                //
                // let animationVars = {width: this.hexSideSize, height: hexHeight};
                // if (absPosition <= 2) {
                //     animationVars['left'] = leftOffset;
                //     animationVars['top'] = topOffset;
                // }
                // timeline.to(hexagonMain, 0.4, animationVars);
                //
                // this.hexTextStyle.marginLeft = (hexHightToSideRatio - 1) * this.hexSideSize / -2 + 'px';
                // this.hexTextStyle.width = hexHightToSideRatio * this.hexSideSize + 'px';
                // this.hexTextStyle.fontSize = this.hexSideSize / 5 + 7 + 'px';
                //
                // this.hexTextStyle.top = '50%';
            },
            unblockScroll() {
                    this.scrollBlock = false;
            },
            handleScroll(event) {
                if (!this.scrollBlock) {
                    this.scrollBlock = true;
                    let direction = 1;

                    if (event.deltaY > 0) {
                        direction = -1;
                    }
                    let marginValue = (this.hexPosition - this.hexId) * direction;
                    setTimeout(this.unblockScroll, 200);
                    if (marginValue != 2) {
                        this.hexPosition += direction
                        this.changeHexagones();
                    }
                }
            }
        }
    }


</script>

<style scoped>

    .hexagon-main {
        /*background: red;*/
        width: 100px;
        height: 100px;
        opacity: 0;
        position: absolute;
        /*transition: margin 0.5s linear;*/
        /*transition-duration: 0.3s;*/
        z-index: 100;
    }

    .hexText {
        z-index: 10;
        pointer-events: none;
        /*background: blue;*/
        /*opacity: .5;*/
        position: absolute;
        top: 50%;
        margin: 0;
        -ms-transform: translateY(-50%);
        transform: translateY(-50%);
    }

    .hex:hover {
        cursor: pointer;
    }
    .hex {
        /*opacity: .5;*/
        /*transition-duration: 0.3s;*/
        position: absolute;
        /*width: 10em;*/
        /*height: 17.32em;*/

        width: 100%;
        height: 100%;
         border-radius: 9px/ 3px;
        /*background: white;*/
        /*background: yellow;*/
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

    .right-team {
        width: 100px;
        height: 100px;
        background: green;
        position: absolute;
        margin-left: 100px;
    }

    .stadium-name {
        font-size: 24px;
    }

    .event-time {
        font-size: 18px;
    }

    .buy-ticket-button {
        font-size: 16px;
        border-radius: 7px;
        border: 2px solid black;
        width: 45%;
        margin: 10px auto;
        padding: 9px;
    }

</style>