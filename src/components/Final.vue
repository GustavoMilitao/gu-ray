<template>
  <div class="main">
    <video v-if="opened" autoplay muted loop id="myVideo">
      <source src="../assets/eu/nos.mp4" type="video/mp4" />
    </video>
    <audio v-if="opened" autoplay>
      <source src="../assets/eu/nos.mp3" type="audio/mpeg" />
    </audio>
    <div class="title" v-if="opened">
      <v-card class="mx-auto" color="#D32F2F" dark max-width="300">
        <v-card-title>
          <v-icon large left> mdi-calendar-heart </v-icon>
          <span class="text-h6 font-weight-light">Nosso dia 16</span>
        </v-card-title>

        <v-card-text class="text-h5 font-weight-bold">
          Nesse dia completamos a primeira página desse livro que estamos
          escrevendo juntos.
        </v-card-text>

        <v-card-text class="text-h5 font-weight-bold"> Amo você! </v-card-text>

        <v-card-actions>
          <v-list-item class="grow">
            <v-list-item-avatar color="darken-3">
              <v-img
                class="elevation-6"
                alt=""
                src="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"
              ></v-img>
            </v-list-item-avatar>
            <v-icon large left> mdi-cards-heart </v-icon>
            <v-list-item-avatar color="darken-3">
              <v-img
                class="elevation-6"
                alt=""
                src="https://avataaars.io"
              ></v-img>
            </v-list-item-avatar>
          </v-list-item>
        </v-card-actions>
      </v-card>
    </div>
    <div class="container" v-on:click="startVideo()" v-if="!opened">
      <v-card class="mx-auto" color="#D32F2F" dark max-width="400">
        <v-card-title>
          <span class="text-h6 font-weight-light">Clique no livro de fotos</span>
        </v-card-title>
      </v-card>
      <v-card class="mx-auto" max-width="500">
        <img src="../assets/eu/nos.jpg" />
        <img src="../assets/eu/nos.jpg" />
        <img src="../assets/eu/nos.jpg" />
        <img src="../assets/eu/nos.jpg" />
      </v-card>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    question: {},
    nextStep: Function,
  },
  data: () => {
    return {
      opened: false,
    };
  },
  methods: {
    startVideo() {
      this.opened = true;
    },

    selectAnswer: function (card) {
      if (card.color.toLowerCase() === "error") {
        this.$emit(
          "wrong-answer",
          this.msgs[Math.floor(Math.random() * this.msgs.length)]
        );
      } else {
        this.$emit("select-answer", card);
      }
    },
  },
};
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url("../assets/eu/nos.jpg");
}
.container {
  position: relative;
  width: 50%;
  height: 340px;
  margin-top: 100px;
  background: rgba(0, 0, 0, 0);
  transform: rotate(-30deg) skew(25deg) scale(0.8);
  transition: 0.5s;
}
.container img {
  position: absolute;
  width: 100%;
  transition: 0.5s;
}
.container:hover img:nth-child(4) {
  transform: translate(160px, -160px);
  opacity: 1;
}
.container:hover img:nth-child(3) {
  transform: translate(120px, -120px);
  opacity: 0.8;
}
.container:hover img:nth-child(2) {
  transform: translate(80px, -80px);
  opacity: 0.6;
}
.container:hover img:nth-child(1) {
  transform: translate(40px, -40px);
  opacity: 0.4;
}

/* Style the video: 100% width and height to cover the entire window */
#myVideo {
  position: fixed;
  min-width: 100%;
  min-height: 100%;
}

/* Add some content at the bottom of the video/page */
.content {
  position: fixed;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  color: #f1f1f1;
  width: 100%;
  padding: 20px;
}
</style>
