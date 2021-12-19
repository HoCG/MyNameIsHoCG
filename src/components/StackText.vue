<template>
    <canvas class="the-canvas"></canvas>
</template>
<script>
    export default {
        mounted() {
            let canvas = document.querySelector('.the-canvas');
            let context = canvas.getContext('2d');
            let ratio = window.devicePixelRatio || 1;

            let totalLineHeight = 560;
            let totalLines = 4;
            let totalDiff = totalLineHeight / totalLines;
            let fontHeight = 60 * ratio - 50; // Small centering

            let smallestWidth = 280; // width of smallest line;
            let offsetX = 12;
            let offsetY = 6;
            let iterations;
            let verticalAlign,
                line1Diff,
                line2Diff,
                line3Diff,
                line4Diff,
                iteration,
                animationFrame;
            let startRGB = [255, 255, 255];
            let endRGB = [220, 165, 163];
            let fullColorSet = [];

            init();

            function init() {

                // Cancel any already running animations
                cancelAnimationFrame(animationFrame);

                // Set the canvas width and height
                canvas.width = window.innerWidth * ratio;
                canvas.height = window.innerHeight * ratio;

                // Set the canvas font properties
                context.font = '180px Montserrat';
                context.border = "800";
                context.textAlign = 'center';
                context.fillStyle = '#fff';
                context.strokeStyle = "#F0A5A3";
                context.lineWidth = "3";
                context.textBaseline = "middle";

                // Centering of the text
                verticalAlign = (window.innerHeight / 2 * ratio) - totalLineHeight / 2;
                line1Diff = totalLineHeight + fontHeight - totalDiff;
                line2Diff = totalLineHeight + fontHeight - totalDiff * 2;
                line3Diff = totalLineHeight + fontHeight - totalDiff * 3;
                line4Diff = totalLineHeight + fontHeight - totalDiff * 4;

                // How many iterations will we go through?
                iterations = Math.floor(
                    ((window.innerWidth * ratio / 2) - (smallestWidth * ratio / 2)) / offsetX + 5
                );
                prepareColorSets(iterations);
                iteration = 0;
                animationFrame = requestAnimationFrame(draw);
            }

            // Draw loop
            function draw() {
                context.clearRect(0, 0, canvas.width, canvas.height);
                for (let i = iterations - 1; i > 0; i--) {
                    context.fillStyle = 'rgb(' + fullColorSet[i][0] + ',' + fullColorSet[i][1] +
                            ',' + fullColorSet[i][2] + ')';
                    let x = window.innerWidth / 2 * ratio - i * offsetX;
                    let y = verticalAlign + i * offsetY + (Math.sin(i + iteration) * 2);
                    drawText(x, y);
                }
                iteration += 0.1;
                animationFrame = requestAnimationFrame(draw);
            }

            // Draw the single lines of text.
            function drawText(x, y) {
                context.fillText("The Most", x, y + line4Diff);
                context.strokeText("The Most", x, y + line4Diff);

                context.fillText("Important", x, y + line3Diff);
                context.strokeText("Important", x, y + line3Diff);

                context.fillText("Programmer", x, y + line2Diff);
                context.strokeText("Programmer", x, y + line2Diff);

                context.fillText("HoCG", x, y + line1Diff);
                context.strokeText("HoCG", x, y + line1Diff);
            }

            // We do this so we don't have to calculate these EVERY loop.
            function prepareColorSets(iterations) {
                fullColorSet = [];
                for (let i = 0; i < iterations; i++) {
                    fullColorSet.push(colourGradientor(1 - i / iterations, startRGB, endRGB));
                }
            }

            // THNX -
            // http://stackoverflow.com/questions/14482226/how-can-i-get-the-color-halfway-between-two-colors
            function colourGradientor(p, rgb_beginning, rgb_end) {

                let w = p * 2 - 1;
                let w1 = (w + 1) / 2.0;
                let w2 = 1 - w1;
                let rgb = [
                    parseInt(rgb_beginning[0] * w1 + rgb_end[0] * w2),
                    parseInt(rgb_beginning[1] * w1 + rgb_end[1] * w2),
                    parseInt(rgb_beginning[2] * w1 + rgb_end[2] * w2)
                ];
                return rgb;
            }
            window.onresize = init;
        }
    }
</script>
<style>
    @import url('https://fonts.googleapis.com/css?family=Montserrat:700');
    canvas {
        width: 50%;
        height: 50%;
        background: #F3BE4E;
    }
</style>