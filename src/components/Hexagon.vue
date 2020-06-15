<template>
    <div class="hexagon-main" :class=" {sideHex: hexPosition}" :style="customStyle"
         ref="hexagonMain">
        <div style="position: absolute; " class="hexText">
            <div class="hes-text-inner">
                <div v-if="!circleIsCentral" class="side-hexagon">
                    <div class="dayShower small-hex">
                        {{eventData.day}}
                    </div>
                    <div class="monthShower">
                        {{eventData.month}}
                    </div>
                </div>

                <div v-else>
                    <div class="central-hexagon-data">
                        <div class="upper-center-part">
                            <div class="stadium-name">
                                {{eventData.stadium}}
                            </div>
                            <div class="day-monthShower">
                                {{eventData.day}} {{eventData.month | uppercase}}
                            </div>
                        </div>
                        <div class="lower-central-part">
                            <div class="event-time">
                                {{eventData.time}}
                            </div>
                            <div class="buy-ticket-button">
                                Купить билет
                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {TimelineLite} from 'gsap'

    export default {
        name: "Hexagon",
        data: function () {
            return {
                gsapObject: new TimelineLite(),
                circleIsCentral: false,
                oneSideElementAmount: 2,
                count: 1,
                scrollBlock: false,
                hexPosition: this.pos,
                hexId: this.hexid,
                customStyle: {},
                scrollTime: 300,
                halfScrollTime: 150,
                scrollTimeSec: 0.3
            }
        },
        filters: {
            uppercase: function (v) {
                return v.toUpperCase();
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
            this.halfScrollTime = this.scrollTime / 2;
            this.scrollTimeSec = this.scrollTime / 1000;
            this.changeHexagons(false);
        },

        methods: {
            makeCircleCentral() {
                if (!this.hexPosition) {
                    this.circleIsCentral = true;
                }
            },
            /**
             * Изменяет параметры гексагона в зависимости от позиции, которую он занимает сейчас
             *
             * @param animate  Устанавливает, отображать ли изменения параметров плавно
             */
            changeHexagons(animate = true) {

                let absPosition = Math.abs(this.hexPosition);

                const {hexagonMain} = this.$refs

                let paramsByPosition, paramsByAbsPosition;

                let paramsListByPosition = this.hexParamsByPosition;
                let paramsListByAbsPosition = this.hexParamsByAbsPosition;

                let animationVars = {};

                let newOpacity = 1;
                if (absPosition <= this.oneSideElementAmount) {
                    paramsByPosition = paramsListByPosition[this.hexPosition];
                    paramsByAbsPosition = paramsListByAbsPosition[absPosition];
                    animationVars = {
                        width: paramsByAbsPosition.sideLength,
                        height: paramsByAbsPosition.height,
                        left: paramsByPosition.mainLeftOffset,
                        top: paramsByPosition.mainTopOffset,
                        fontSize: paramsByAbsPosition.fontSize
                    };
                } else {
                    newOpacity = 0;
                }

                if (animate) {
                    this.circleIsCentral = false;
                    if (!absPosition) {
                        setTimeout(this.makeCircleCentral, this.halfScrollTime);
                    }

                    animationVars['duration'] = this.scrollTimeSec;
                    animationVars['opacity'] = newOpacity;

                    this.gsapObject.to(hexagonMain, animationVars);

                } else {
                    if (!absPosition) {
                        this.circleIsCentral = true;
                    }

                    if (newOpacity) {
                        this.customStyle = {
                            width: paramsByAbsPosition.sideLength + 'px',
                            height: paramsByAbsPosition.height + 'px',
                            left: paramsByPosition.mainLeftOffset + 'px',
                            top: paramsByPosition.mainTopOffset + 'px',
                            fontSize: paramsByAbsPosition.fontSize + 'px'
                        };
                    }
                }
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
                    setTimeout(this.unblockScroll, this.scrollTime);
                    if (marginValue !== this.oneSideElementAmount) {
                        this.hexPosition += direction
                        this.changeHexagons();
                    }
                }
            }
        }
    }


</script>

<style scoped>

</style>