<template>
  <v-container>
    <v-row class="text-center">
      <div class="image-drag">
        <div class="image-drag__inner"></div>
      </div>
      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>
        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a
            href="https://community.vuetifyjs.com"
            target="_blank"
          >Discord Community</a>
        </p>
      </v-col>
      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          What's next?
        </h2>
        <v-row justify="center">
          <a
            v-for="(next, i) in whatsNext"
            :key="i"
            :href="next.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ next.text }}
          </a>
        </v-row>
      </v-col>
      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Important Links
        </h2>
        <v-row justify="center">
          <a
            v-for="(link, i) in importantLinks"
            :key="i"
            :href="link.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ link.text }}
          </a>
        </v-row>
      </v-col>
      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Ecosystem
        </h2>
        <v-row justify="center">
          <a
            v-for="(eco, i) in ecosystem"
            :key="i"
            :href="eco.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ eco.text }}
          </a>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
    import gsap from 'gsap';
    export default {
        name: 'HelloWorld',
        mounted(){
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
        data: () => ({
            ecosystem: [
                {
                    text: 'vuetify-loader',
                    href: 'https://github.com/vuetifyjs/vuetify-loader'
                }, {
                    text: 'github',
                    href: 'https://github.com/vuetifyjs/vuetify'
                }, {
                    text: 'awesome-vuetify',
                    href: 'https://github.com/vuetifyjs/awesome-vuetify'
                }
            ],
            importantLinks: [
                {
                    text: 'Documentation',
                    href: 'https://vuetifyjs.com'
                }, {
                    text: 'Chat',
                    href: 'https://community.vuetifyjs.com'
                }, {
                    text: 'Made with Vuetify',
                    href: 'https://madewithvuejs.com/vuetify'
                }, {
                    text: 'Twitter',
                    href: 'https://twitter.com/vuetifyjs'
                }, {
                    text: 'Articles',
                    href: 'https://medium.com/vuetify'
                }
            ],
            whatsNext: [
                {
                    text: 'Explore components',
                    href: 'https://vuetifyjs.com/components/api-explorer'
                }, {
                    text: 'Select a layout',
                    href: 'https://vuetifyjs.com/getting-started/pre-made-layouts'
                }, {
                    text: 'Frequently Asked Questions',
                    href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions'
                }
            ]
        })
    }
</script>
<style lang="scss" > 
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
        background-image: url("../assets/profileIMG.jpg") !important;
        background-size: 100%;
        background-position: 50%;
        background-repeat: none;
    }
}
</style>