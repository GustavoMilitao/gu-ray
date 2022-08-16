<template>
  <v-app>
    <v-main>
      <div class="text-center" v-if="carregandoFinal && !showFinal">
        <v-progress-circular
          :rotate="-90"
          :size="350"
          :width="15"
          :value="value"
          color="primary"
        >
          {{ getLoadingMsg() }}<br>
          {{ value + '%'  }}
        </v-progress-circular>
      </div>
      <div class="text-center ma-2" v-if="!carregandoFinal && !showFinal">
        <v-snackbar color="red accent-2" v-model="snackbar" :timeout="timeout">
          {{ snackbarMessage }}
        </v-snackbar>
      </div>
      <StepperFirstMonth
        v-if="!carregandoFinal && !showFinal"
        v-on:wrong-answer="wrongAnswer"
        v-on:finish="finish"
      />
      <Final v-if="!carregandoFinal && showFinal"> </Final>
    </v-main>
  </v-app>
</template>

<script>
import StepperFirstMonth from "./components/StepperFirstMonth";
import Final from "./components/Final";

export default {
  name: "App",

  components: {
    StepperFirstMonth,
    Final
  },

  beforeDestroy() {
    clearInterval(this.interval);
  },

  methods: {
    wrongAnswer(message) {
      this.snackbarMessage = message;
      this.snackbar = true;
    },

    getLoadingMsg() {
      return `${this.loadingMsgs[
        Math.floor(Math.random() * this.loadingMsgs.length)
      ]}`;
    },

    finish() {
      this.carregandoFinal = true;
      this.interval = setInterval(() => {
        if (this.value === 100) {
          this.carregandoFinal = false;
          this.showFinal = true;
          return (this.value = 0);
        }
        this.value += 10;
      }, 1000);
    },
  },

  data: () => ({
    snackbarMessage: "",
    snackbar: false,
    timeout: 1000,
    carregandoFinal: false,
    showFinal: false,
    interval: {},
    value: 0,
    loadingMsgs: [
      "CALMA AINDA",
      "SOMA DAQUI, SOMA DE LA, PERA AINDA",
      "MAIS UM TIKIN",
      "TÁ ACABANDO",
      "CALCULANDO, GUENTA A MÃO",
      "NÃO CRIEMOS PÂNICO",
      "TÔ FAZENDO UNS CÁLCULO, SIACALME",
      "RELAXA",
      "QUAAAASEEE LÁAA",
      "CALCULANDO PONTUAÇÃO",
      "CALCULANDO",
    ],
  }),
};
</script>

<style scoped>
.v-progress-circular {
  margin: 1rem;
}
</style>