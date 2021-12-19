<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-img contain height="300px" width="300px" class="profileImage" :src="this.ProfileIMG"></v-img>
      </v-col>
      <div class="mainText">
        <h1>Name: 황호세</h1><br>
        <h1>Age: 25</h1>
      </div>
      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
        </v-row>
      </v-col>
      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
        </v-row>
      </v-col>
      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
    export default {
        name: 'HelloWorld',
        mounted(){
        },
        data: () => ({
            ProfileIMG: require("../assets/profileIMG.jpg")
        })
    }
</script>
<style lang="scss">
.mainText {
  text-align: left;
} 
.profileImage {
  float: left;
  filter: grayscale(100%);
  position: absolute;
}
</style>