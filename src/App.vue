<template>
    <v-app>
        <v-main>
            <div class="image-drag">
                <div class="image-drag__inner"></div>
            </div>
            <MainView/>
            <StackText/>
        </v-main>
    </v-app>
</template>

<script>
    import MainView from './views/MainView.vue';
    import StackText from './components/StackText.vue';
    import gsap from 'gsap';
    export default {
        name: 'App',
        mounted() {
            console.clear();
            const images = document.querySelectorAll(".image-drag");
            images.forEach(image => {
                let isDown = false;
                let innerImage = image.children[0];
                image.addEventListener("mousedown", e => {
                    isDown = true;
                    image.style.cursor = "grabbing";
                    scaleImageBackground(innerImage, isDown);
                    scaleImage(innerImage, isDown);
                    mousedown(e, image);
                });

                image.addEventListener("mouseup", () => {
                    isDown = false;
                    image.style.cursor = "grab";
                    scaleImageBackground(innerImage, isDown);
                    scaleImage(innerImage, isDown);
                });
            });

            function mousedown(e, image) {
                window.addEventListener("mousemove", mousemove);
                window.addEventListener("mouseup", mouseup);

                e.target.parentElement.style.zIndex = 10;
                images.forEach(i => {
                    if (i != e.target.parentElement) {
                        i.style.zIndex = 1;
                    }
                });

                let prevX = e.clientX;
                let prevY = e.clientY;

                function mousemove(e) {
                    let newX = prevX - e.clientX;
                    let newY = prevY - e.clientY;
                    moveImage(image, {
                        x: newX,
                        y: newY
                    });
                    prevX = e.clientX;
                    prevY = e.clientY;
                }
                function mouseup() {
                    window.removeEventListener("mousemove", mousemove);
                    window.removeEventListener("mouseup", mouseup);
                }
            }

            function scaleImageBackground(image, mouseDownState) {
                let duration = 0.45;
                if (mouseDownState) {
                    gsap.to(image, duration, {
                        backgroundSize: "150%",
                        ease: "power3.out"
                    });
                } else {
                    gsap.to(image, duration, {
                        backgroundSize: "120%",
                        ease: "power3.out"
                    });
                }
            }

            function scaleImage(image, mouseDownState) {
                let duration = 0.45;
                if (mouseDownState) {
                    gsap.to(image, duration, {
                        scale: 0.85,
                        ease: "power3.out"
                    });
                } else {
                    gsap.to(image, duration, {
                        scale: 1,
                        ease: "power3.out"
                    });
                }
            }

            function moveImage(image, mousePosition) {
                let imageBounds = image.getBoundingClientRect();
                let newPosition = {
                    left: imageBounds.left - mousePosition.x,
                    top: imageBounds.top - mousePosition.y
                };

                gsap.to(image, 0, {
                    top: `${newPosition.top}px`,
                    left: `${newPosition.left}px`,
                    ease: "power3.out"
                });
            }
        },
        components: {
            MainView,
            StackText
        },

        data: () => ({
            //
        })
    };
</script>
<style lang="scss">
    .image-drag {
        position: absolute;
        width: 240px;
        height: 320px;
        cursor: grab;
        &__inner {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            filter: grayscale(100%);
            background-image: url("./assets/logo.png") !important;
            background-size: 120%;
            background-position: 60%;
            background-repeat: none;
        }
    }
</style>