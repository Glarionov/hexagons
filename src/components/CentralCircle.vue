<template>
    <div class="central-circles" ref="centralCircle">
    </div>
</template>

<script>
    export default {
        name: "CentralCircle",
        methods: {
            /**
             * Устанавливает параметры центральной окружности так, чтобы она касалась определённой точки гексагона
             *
             * @param hexagonSizes      Размеры гексагона
             * @param hexagonOffsets    Данные положения гексагона
             * @param whatToTouch       Точка гексагона, которой должна касаться окружность
             * @returns {{borderRadius: string, width: string, marginTop: string, marginLeft: string, height: string}}
             */
            setCentralCircleParamsByHexagon(hexagonSizes, hexagonOffsets, whatToTouch = 'center') {
                switch (whatToTouch) {
                    case 'corner':
                        this.setCentralCircleParamsToTouchPoint(hexagonOffsets.mainLeftOffset + hexagonSizes.sideLength,
                            hexagonOffsets.mainTopOffset + hexagonSizes.height
                        );
                        break;

                    case 'center':
                        this.setCentralCircleParamsToTouchPoint(hexagonOffsets.mainLeftOffset + hexagonSizes.sideLength / 2,
                            hexagonOffsets.mainTopOffset + hexagonSizes.height / 2
                        );
                        break;
                }

            },
            /**
             * Устанавливает параметры центральной окружности так, чтобы она касалась точками с координатами x, y
             *
             * @param x
             * @param y
             */
            setCentralCircleParamsToTouchPoint(x, y) {
                let halfWindowWidth = window.innerWidth / 2;
                let halfWindowHeight = window.innerHeight / 2;

                let innerCircleRadius = x - halfWindowWidth;
                let yDiffBetweenScreenCenterAndHexBottom = halfWindowHeight - y;
                innerCircleRadius /= Math.cos(Math.atan(yDiffBetweenScreenCenterAndHexBottom / innerCircleRadius));

                let innerCircleDiameter = innerCircleRadius * 2;
                let innerCircleDiameterPx = innerCircleDiameter + 'px';

                this.$refs.centralCircle.style.marginLeft = halfWindowWidth - innerCircleRadius + 'px';
                this.$refs.centralCircle.style.marginTop = halfWindowHeight - innerCircleRadius + 'px';

                this.$refs.centralCircle.style.height =
                this.$refs.centralCircle.style.width =
                this.$refs.centralCircle.style.borderRadius = innerCircleDiameterPx;
            }
        }
    }
</script>

<style scoped>
    .central-circles {
        position: absolute;
        margin: auto;
        border: 1px solid #6C7296;
        z-index: 3;
    }
</style>