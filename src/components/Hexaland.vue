<template>
    <div class="hexaland-main">
<!--        <hexagone/>-->
        <div v-if="false">
            <test/>
        </div>

        <div class="inner-circle">

        </div>
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

        <div v-for="(item,index) in hexagonsData" :key="index" >
            <div class="hexagon-main-wrapper" ref="hexwrapper"  v-on:click="rollToCenter(index)" >
                <Hexagone
                        :hexParamsByPosition="hexParamsByPosition"
                        :hexParamsByAbsPosition="hexParamsByAbsPosition"
                        :eventData="item" :pos="index - oneSideElementAmount" :hexid="index - oneSideElementAmount"
                 ref="hexInclusion" />
            </div>

        </div>
                <div class="top-menu">
            TTTTTTT
        </div>
    </div>
</template>

<script>
    import Hexagone from "./Hexagone";
    import Test from "./Test";

    export default {
        created(){
            window.addEventListener('wheel', this.handleScroll);
            window.addEventListener('resize', this.handleResize);
                        this.$nextTick(() => {
                window.addEventListener('resize', this.handleResize);
            })

            this.setTeams();

        },
                mounted (){
            this.handleResize();
            this.$nextTick(() => {
                window.addEventListener('resize', this.handleResize);
            })
                },
        props:{
            selectedTeams1: Object,
        },
        name: "Hexaland",
        components: {
            Hexagone,
            Test
        },
        methods: {
            unblockScroll() {
                console.log('unblock scroll')
                this.scrollBlock = false;
            },
            setTeams() {
                let currentTeams = this.hexagonsData[this.currentCentralElement]['teams'];
                let leftTeam = this.teams[currentTeams[0]];
                let rightTeam = this.teams[currentTeams[1]];
                /*g*/console.log('leftTeam'); //todo remove it
                /*g*/console.log(leftTeam); //todo remove it
                /*g*/console.log('rightTeam'); //todo remove it
                /*g*/console.log(rightTeam); //todo remove it
                this.leftTeam = leftTeam;
                this.rightTeam = rightTeam;
            },
            handleScroll(event) {
                let ff = false;
                /*g*/console.log('ff'); //todo remove it
                /*g*/console.log(ff); //todo remove it
                if (!this.scrollBlock) {
                    this.scrollBlock = true;
                    let direction = 1;

                    if (event.deltaY > 0) {
                        direction = -1;
                    }
                    /*g*/console.log('direction'); //todo remove it
                    /*g*/console.log(direction); //todo remove it
                    let marginValue = (this.currentCentralElement - this.oneSideElementAmount) * direction;
                    /*g*/console.log('marginValue'); //todo remove it
                    /*g*/console.log(marginValue); //todo remove it
                    setTimeout(this.unblockScroll, 200);
                    if (marginValue != -2) {
                        this.currentCentralElement -= direction;
                        this.setTeams();
                    }
                }
            },

            setHexSizesAfterWindowResize() {
                this.windowHeight = window.innerHeight
                this.windowWidth = window.innerWidth
                this.halfWindowWidth = this.windowWidth / 2
                let halfWindowWidth = this.windowWidth / 2
                this.halfWindowHeight = this.windowHeight / 2;
                let halfWindowHeight = this.windowHeight / 2;

                let absPosition = Math.abs(this.hexPosition);

                let widthLimit = 1200;

                let currentWidthWithLimit = this.windowWidth;
                if (currentWidthWithLimit > widthLimit) {
                    currentWidthWithLimit = widthLimit;
                }
                this.centralHexSideSize = currentWidthWithLimit / (8.5);
                let centralHexSideLength = currentWidthWithLimit / (8.5);


                // let halfOfDifferenceBetweenHeightAndSide = 0.366;
                let sizeProportionsToZero = [1, 0.6, 0.4];

                let marginProportions = [0, 0.9, 1.5];
                // this.ptz = sizeProportionsToZero[absPosition];


                let hexSideLength, hexHeight;

                //this.hexTextStyle.fontSize = this.hexSideSize / 5 + 7 + 'px';




                /*
                    Вычисляются параметры для шестиугольников с модулем индекса позиции 0-N,
                    Которые совпадают у элементов, с противоположным индексом
                */

                for (let absPosition = 0; absPosition <= this.oneSideElementAmount; absPosition++) {
                    hexSideLength = centralHexSideLength * sizeProportionsToZero[absPosition];
                    this.hexParamsByAbsPosition[absPosition] =
                        {
                            sideLength: hexSideLength,
                            height: hexSideLength * this.mathConstants.hexHeightToSideRatio,
                            textMarginLeft: (this.mathConstants.hexHeightToSideRatio - 1) * hexSideLength / -2,
                            fontSize: hexSideLength / 6 + 7
                        };
                }

                /*g*/console.log('00000000this.hexParamsByAbsPosition'); //todo remove it
                /*g*/console.log(this.hexParamsByAbsPosition); //todo remove it

                let mainLeftOffset, mainTopOffset, extraMarginBase;

                console.log(172+"\n"); //todo remove it
                /*
                    Вычисляются параметры для шестиугольников с индексами позиции [-N; N]
                */
                for (let position = this.oneSideElementAmount * -1; position <= this.oneSideElementAmount; position++) {
                    let a = 4;

                    console.log(a+"\n"); //todo remove it
                    absPosition = Math.abs(position);

console.log(179+"\n"); //todo remove it

                    hexSideLength = this.hexParamsByAbsPosition[absPosition]['sideLength'];
                    hexHeight = this.hexParamsByAbsPosition[absPosition]['height'];

                    mainLeftOffset = halfWindowWidth - hexSideLength / 2;
                    mainTopOffset = halfWindowHeight - hexHeight / 2;
console.log(186+"\n"); //todo remove it
                    /*
                        Вычисляются дополнительные отклонения от центра
                        Шестиугольник с индексом 0 - центральный, для него делать вычисления не нужно
                    */

                    if (position) {
                        extraMarginBase = marginProportions[absPosition] * centralHexSideLength * position / absPosition;

                        mainLeftOffset += extraMarginBase * this.mathConstants.hexHeightToSideRatio ;
                        mainTopOffset -= extraMarginBase;
                    }

console.log(197+"\n"); //todo remove it
                    this.hexParamsByPosition[position] = {
                        mainLeftOffset: mainLeftOffset,
                        mainTopOffset: mainTopOffset
                    };

                    /*g*/console.log('this.$refs.hexInclusion'); //todo remove it
                    /*g*/console.log(this.$refs.hexInclusion); //todo remove it
                    this.$refs.hexInclusion[position + this.oneSideElementAmount].showHexagone();
                    this.$refs.hexInclusion[position + this.oneSideElementAmount].changeHexagones(false);
                }

console.log(203+"\n"); //todo remove it
                /*g*/console.log('111111this.hexParamsByPosition'); //todo remove it
                /*g*/console.log(this.hexParamsByPosition); //todo remove it

            },
            handleResize() {

                this.setHexSizesAfterWindowResize();
                console.log(89+"\n"); //todo remove it
                this.windowHeight = window.innerHeight
                this.windowWidth = window.innerWidth
                /*g*/console.log('this.windowWidth'); //todo remove it
                /*g*/console.log(this.windowWidth); //todo remove it
                this.halfWindowWidth = this.windowWidth / 2
                this.halfWindowHeight = this.windowHeight / 2;
// let hexHightToSideRatio = 1.732;

let hexHightToSideHalfRatio = 0.866;

                // let absPosition = Math.abs(this.hexPosition);

                let widthLimit = 1200;
                let currentWidthWithLimit = this.windowWidth;
                if (currentWidthWithLimit > widthLimit) {
                    currentWidthWithLimit = widthLimit;
                }
                this.centralHexSideSize = currentWidthWithLimit / (8.5);

                                let hexHalfHeight = Math.floor(this.centralHexSideSize * hexHightToSideHalfRatio);
                let hexFourthHeight = hexHalfHeight / 2;

                let hh = hexHalfHeight + 'px';


                let rightTeamMarginLeft = this.halfWindowWidth +this.centralHexSideSize * 1.2 + hexFourthHeight / 2;
                let rightTeamMarginLeftPx = rightTeamMarginLeft + 'px' ;



                // let mmr = this.halfWindowHeight + this.centralHexSideSize * hexHightToSideRatio/2 + 'px';
                let halfWindowHeightPx = this.halfWindowHeight  + 'px';


                this.rightTeamStylePartBefore.marginLeft = rightTeamMarginLeft - hexFourthHeight + 'px';
                this.rightTeamStylePartBefore.marginTop = halfWindowHeightPx;

                this.rightTeamStyle.marginLeft = rightTeamMarginLeftPx;

                this.rightTeamStyle.marginTop = halfWindowHeightPx;
                this.rightTeamStyle.height = hh;
                this.rightTeamStyle.width = this.windowWidth - rightTeamMarginLeft + 'px';

                let leftSideWidth = this.windowWidth - rightTeamMarginLeft;
                let leftSideWidthPx = leftSideWidth + 'px';
                this.leftTeamStyle.width = leftSideWidthPx;
                /*g*/console.log('this.leftTeamStyle.width'); //todo remove it
                /*g*/console.log(this.leftTeamStyle.width); //todo remove it
                this.leftTeamStyle.height = hh;
                let leftSideMarginTop = this.halfWindowHeight - this.centralHexSideSize * hexHightToSideHalfRatio + 'px';
                this.leftTeamStyle.marginTop = leftSideMarginTop;

                this.leftTeamStylePartAfter.marginTop = leftSideMarginTop;
                this.leftTeamStylePartAfter.height = hh;
                this.leftTeamStylePartAfter.width = hh;
                this.leftTeamStylePartAfter.marginLeft = leftSideWidth - hexFourthHeight + 'px';


                this.rightTeamStylePartBefore.width = hh;
                this.rightTeamStylePartBefore.height = hh;


                this.leftTeam += '1';
                this.setTeams();

            },
            roll2()
    {
        console.log('roll2')
    }
    ,
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
                            hexComponent.changeHexagones();
                        }
                    );
                }
                let componentId = clickedComponent.hexId + this.oneSideElementAmount;
                this.currentCentralElement = componentId;
                this.setTeams();
            }
    },
        data: function () {
            return {
                hexParamsByAbsPosition: [],
                hexParamsByPosition: {},
                mathConstants: {
                    squareDiagonalToSide: 1.414,
                    insideHelpingSquareSideToOuterSquare: 1.15470053839,
                    oneMinusSin60: 0.13397459622,
                    cos45minusCos75: 0.44828773608,
                    hexHeightToSideRatio: 1.732,
                    hexHeightToSideHalfRatio: 0.866
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
                teams: {
                  100: 'Спартак',
                  200: 'Рубин',
                  300: 'Красные Драконы',
                  400: 'Ягуары'
                },
                hexagonsData: [
                    {
                        time: '11:00',
                        day: 30,
                        month: 'мая',
                        teams: [100, 300],
                        stadium: 'Лужники'
                    },
                    {
                        time: '14:00',
                        day: 17,
                        month: 'июня',
                        teams: [200, 300],
                        stadium: 'Газпром-арена'
                    }, {
                    time: '19:00',
                        day: 26,
                        month: 'июня',
                        teams: [100, 200],
                        stadium: 'Олимпийский'
                    }, {
                    time: '21:00',
                        day: 16,
                        month: 'июля',
                        teams: [100, 400],
                        stadium: 'Ак барс арена'

                    }, {
                    time: '22:30',
                        day: 30,
                        month: 'сентября',
                        teams: [300, 400],
                        stadium: 'Арена Химки'
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
        /*background: radial-gradient(circle at center, white -32%, #181E3E, #000000);*/
            background: radial-gradient(circle at center,#4D5083 30%, #181E3E 64%, #000002 81%);
        overflow: hidden;
    }

    .side-part {
        background: red;
        position: absolute;
    }

    .left-side-part::after {
        width: inherit;
        height: inherit;
        background: inherit;
                width: 100px;
        height: 100px;
        background: blue;
    }

    .left-side-part-after {
        position: absolute;
        overflow: hidden;
        z-index: 200;


        border-top-right-radius: 9px;
    border-bottom-right-radius: 5px;

        /*width: 100px;*/
        /*height: 100px;*/
        /*background: green;*/
    }

    .right-side-part-before, .left-side-part-after {
        transform: skew(-30deg, 0deg);
    }

    .right-side-part-before {
        background: white;
        border-top-left-radius: 9px;
        border-bottom-left-radius: 5px;
    }

    .side-part-triangle-inside {
        position: absolute;
        transform: rotate(30deg);
        border-top-right-radius: 12px;
        width: 200px;
        height: 200px;
        display: none;
    }

    .side-part-triangle-inside, .side-part, .hex, side-part-triangle-inside, .left-side-part-after {
        background: white;
        position: absolute;

        /*border-bottom-right-radius: 22px;*/
    }

    .triangle-helper {
        position: absolute;
    }

    .left-side-part {
        z-index: 40;
        /*border-bottom-right-radius: 13px;*/
    }

    .team-name-span-wrapper {
        display: table;
        height: 100px;
        width: 90%;
        /*text-align: center;*/
    }

    .team-name-span {
        display: table-cell;
        vertical-align: middle;
        horiz-align: right;

        font-size: 30px;
    }

    .left-side-part .team-name-span {
        text-align: right;
    }

    .right-side-part .team-name-span {
        text-align: left;
    }

    .inner-circle {
        position: absolute;
        margin: auto;
        width: 400px;
        height: 400px;
        background: red;
        opacity: 0.5;
    }

</style>