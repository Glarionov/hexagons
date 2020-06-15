<template>
    <div class="hexaland-main">
        <CentralCircle ref="innerCircle"/>
        <CentralCircle ref="outerCircle"/>
        <div class="side-part left-side-part" :style="leftTeamStyle">
            <div class="team-name-span-wrapper">
                <div class="team-name-span">
                    {{leftTeam}}
                </div>
            </div>

        </div>
        <div class="left-side-part-after triangle-helper" :style="leftTeamStylePartAfter">
        </div>

        <div class="right-side-part-before triangle-helper" :style="rightTeamStylePartBefore">
        </div>
        <div class="side-part right-side-part" :style="rightTeamStyle">
            <div class="team-name-span-wrapper">
                <div class="team-name-span">
                    {{rightTeam}}
                </div>
            </div>


        </div>

        <div v-for="(item,index) in hexagonsData" :key="index">
            <div class="hexagon-main-wrapper" ref="hexwrapper" v-on:click="rollToCenter(index)">
                <Hexagon
                        :hexParamsByPosition="hexParamsByPosition"
                        :hexParamsByAbsPosition="hexParamsByAbsPosition"
                        :eventData="item" :pos="index - oneSideElementAmount" :hexid="index - oneSideElementAmount"
                        :oneSideElementAmount="oneSideElementAmount"
                        ref="hexInclusion"/>
            </div>

        </div>
        <TopMenu/>
    </div>
</template>

<script>
    import Hexagon from "./Hexagon";
    import CentralCircle from "./CentralCircle";
    import TopMenu from "./TopMenu";

    import {eventData} from '../assets/js/eventData';
    import {teamsData} from '../assets/js/teamsData';

    export default {
        name: "mainPage",
        components: {
            Hexagon,
            CentralCircle,
            TopMenu
        },
        created() {

            this.hexagonsData = eventData;

            this.teams = teamsData;
            window.addEventListener('wheel', this.handleScroll);
            window.addEventListener('resize', this.handleResize);


            this.handleResize(false)
            this.setTeams();

        },
        mounted() {
            this.setCentralCirclesParams();
        },
        props: {
            selectedTeams1: Object,
        },
        methods: {
            unblockScroll() {
                this.scrollBlock = false;
            },
            setTeams() {
                let currentTeams = this.hexagonsData[this.currentCentralElement]['teams'];
                this.leftTeam = this.teams[currentTeams[0]];
                this.rightTeam = this.teams[currentTeams[1]];
            },
            handleScroll(event) {
                if (!this.scrollBlock) {
                    this.scrollBlock = true;
                    setTimeout(this.unblockScroll, 200);

                    let direction = 1;
                    if (event.deltaY > 0) {
                        direction = -1;
                    }

                    let marginValue = (this.currentCentralElement - this.oneSideElementAmount) * direction;
                    if (marginValue !== -1 * this.oneSideElementAmount) {
                        this.currentCentralElement -= direction;
                        this.setTeams();
                    }
                }
            },

            /**
             * Основываясь на параметрах окна,
             * создаёт массив с размерами и отступами для  шестиугольника на каждой позиции, от -2 до 2
             *
             **/
            setHexSizesAfterWindowResize(resize = true) {
                this.halfWindowWidth = window.innerWidth / 2
                let halfWindowWidth = window.innerWidth / 2
                this.halfWindowHeight = window.innerHeight / 2;
                let halfWindowHeight = window.innerHeight / 2;

                let absPosition = Math.abs(this.hexPosition);

                let widthLimit = 1200;

                let currentWidthWithLimit = window.innerWidth;
                if (currentWidthWithLimit > widthLimit) {
                    currentWidthWithLimit = widthLimit;
                }

                let sizeProportionsToZero = [1, 0.57, 0.4];
                let leftMarginRatios = [0, 1.5, 2.45];
                if (window.innerWidth < 600) {
                    sizeProportionsToZero = [1, 0.58, 0.50];
                    leftMarginRatios = [0, 1.3, 2.3];
                }

                let topToLeftOffsetRatio;
                if (window.innerHeight < 600) {
                    topToLeftOffsetRatio = this.numValues.tooOffsetToLeftOffsetLowScreen;
                } else {
                    topToLeftOffsetRatio = this.numValues.tooOffsetToLeftOffsetDefault;
                }

                let centralHexSideLengthToScreenWidth = .125;
                this.centralHexSideSize = Math.floor(currentWidthWithLimit * centralHexSideLengthToScreenWidth);
                let centralHexSideLength = this.centralHexSideSize;

                let topOffsetOfCentralHexagon = halfWindowHeight - centralHexSideLength / 2;
                let hexSideLength, hexHeight;
                let extraOffsetsByAbsPosition = [topOffsetOfCentralHexagon];

                // Вычисляются размеры для шестиугольников с модулем индекса позиции 0-N,
                for (let absPosition = 0; absPosition <= this.oneSideElementAmount; absPosition++) {
                    hexSideLength = centralHexSideLength * sizeProportionsToZero[absPosition];
                    if (absPosition) {
                        extraOffsetsByAbsPosition[absPosition] = extraOffsetsByAbsPosition[absPosition - 1] + 1;
                    }

                    this.hexParamsByAbsPosition[absPosition] =
                        {
                            sideLength: hexSideLength,
                            height: hexSideLength * this.numValues.hexHeightToSideRatio,
                            fontSize: hexSideLength / 7 + 7
                        };
                }

                let mainLeftOffset, mainTopOffset, extraLeftOffset;

                // Вычисляются отступы для шестиугольников с индексами позиции [-N; N]
                for (let position = this.oneSideElementAmount * -1; position <= this.oneSideElementAmount; position++) {
                    absPosition = Math.abs(position);

                    hexSideLength = this.hexParamsByAbsPosition[absPosition].sideLength;
                    hexHeight = this.hexParamsByAbsPosition[absPosition].height;

                    mainLeftOffset = halfWindowWidth - hexSideLength / 2;
                    mainTopOffset = halfWindowHeight - hexHeight / 2;

                    if (position) {
                        extraLeftOffset = leftMarginRatios[absPosition] * centralHexSideLength * position / absPosition;

                        mainLeftOffset += extraLeftOffset;
                        mainTopOffset -= extraLeftOffset * topToLeftOffsetRatio;
                    }

                    this.hexParamsByPosition[position] = {
                        mainLeftOffset: mainLeftOffset,
                        mainTopOffset: mainTopOffset
                    };
                    if (resize) {
                        this.$refs.hexInclusion[position + this.oneSideElementAmount].changeHexagons(false);
                    }
                }

                if (resize) {
                    this.setCentralCirclesParams();
                }
            },
            setCentralCirclesParams() {
                this.$refs.innerCircle.setCentralCircleParamsByHexagon(this.hexParamsByAbsPosition[1], this.hexParamsByPosition[1], 'corner');
                if (this.oneSideElementAmount > 1) {
                    this.$refs.outerCircle.setCentralCircleParamsByHexagon(this.hexParamsByAbsPosition[2], this.hexParamsByPosition[2], 'center');
                }
            },

            /**
             * Устанавливает размеры и отступы боковых полос с названиями команд
             *
             * */
            setTeamBlockStyles() {
                this.halfWindowWidth = window.innerWidth / 2
                this.halfWindowHeight = window.innerHeight / 2;

                let hexHalfHeight = Math.floor(this.centralHexSideSize * this.numValues.hexHeightToSideHalfRatio);
                let hexFourthHeight = hexHalfHeight / 2;
                let hexHalfHeightPx = hexHalfHeight + 'px';
                let rightTeamMarginLeft = this.halfWindowWidth + this.centralHexSideSize * 1.15 + hexFourthHeight / 2;
                let rightTeamMarginLeftPx = rightTeamMarginLeft + 'px';
                let halfWindowHeightPx = this.halfWindowHeight + 'px';

                this.rightTeamStylePartBefore.marginLeft = rightTeamMarginLeft - hexFourthHeight + 'px';
                this.rightTeamStylePartBefore.marginTop = this.rightTeamStyle.marginTop = halfWindowHeightPx;

                this.rightTeamStyle.marginLeft = rightTeamMarginLeftPx;
                this.rightTeamStyle.width = window.innerWidth - rightTeamMarginLeft + 'px';

                let leftSideWidth = window.innerWidth - rightTeamMarginLeft;
                this.leftTeamStyle.width = leftSideWidth + 'px';
                let leftSideMarginTop = this.halfWindowHeight - this.centralHexSideSize * this.numValues.hexHeightToSideHalfRatio + 'px';
                this.leftTeamStyle.marginTop = this.leftTeamStylePartAfter.marginTop = leftSideMarginTop;
                this.leftTeamStylePartAfter.marginLeft = leftSideWidth - hexFourthHeight + 'px';

                this.leftTeamStyle.height =
                    this.rightTeamStyle.height =
                        this.leftTeamStylePartAfter.height =
                            this.leftTeamStylePartAfter.width =
                                this.rightTeamStylePartBefore.width =
                                    this.rightTeamStylePartBefore.height = hexHalfHeightPx;

                this.$forceUpdate();
            },
            handleResize(resize = true) {
                this.setHexSizesAfterWindowResize(resize);
                this.setTeamBlockStyles();
            },
            /**
             * Перемещает гексагоны так, чтобы гексагон с позицией clickedIndex встал в центр
             *
             * @param clickedIndex
             */
            rollToCenter(clickedIndex) {
                let clickedComponent = this.$refs.hexInclusion[clickedIndex];
                if (Math.abs(clickedComponent.hexPosition) >= this.oneSideElementAmount) {
                    clickedIndex -= Math.abs(clickedComponent.hexPosition) - this.oneSideElementAmount;
                }
                clickedComponent = this.$refs.hexInclusion[clickedIndex];
                let direction = -clickedComponent.hexPosition;
                while (clickedComponent.hexPosition) {
                    this.$refs.hexInclusion.forEach(function roller(hexComponent) {
                            hexComponent.hexPosition += direction;
                            hexComponent.changeHexagons();
                        }
                    );
                }
                this.currentCentralElement = clickedComponent.hexId + this.oneSideElementAmount;
                this.setTeams();
            }
        },
        data: function () {
            return {
                hexParamsByAbsPosition: [],
                hexParamsByPosition: {},
                numValues: {
                    squareDiagonalToSide: 1.414,
                    insideHelpingSquareSideToOuterSquare: 1.15470053839,
                    oneMinusSin60: 0.13397459622,
                    cos45minusCos75: 0.44828773608,
                    hexHeightToSideRatio: 1.732,
                    hexHeightToSideHalfRatio: 0.866,
                    tooOffsetToLeftOffsetDefault: 0.6,
                    tooOffsetToLeftOffsetLowScreen: 0.42,
                    hexFieldMaxSize: 1200
                },
                selectedTeams: {},
                leftTeam: '',
                rightTeam: '',
                scrollBlock: false,
                rightTeamStyle: {},
                leftTeamStyle: {},
                leftTeamStylePartAfter: {},
                leftTeamStylePartAfterInside: {},
                rightTeamStylePartBefore: {},
                rightTeamStylePartBeforeInside: {},
                currentCentralElement: 2,
                oneSideElementAmount: 2,
                hexagonsData: {},
                teams: {}
            }
        }
    }
</script>

<style>



</style>