<template>
  <v-card class="mx-auto" max-width="500">
    <v-toolbar color="indigo" dark>
      <v-toolbar-title>{{ question.question }}</v-toolbar-title>
    </v-toolbar>

    <v-container fluid>
      <v-row dense>
        <v-col
          v-for="card in question.cards"
          :key="card.title"
          :cols="card.flex"
        >
          <v-hover v-slot="{ hover }">
            <v-card
              v-on:click="selectAnswer(card)"
              class="card-question"
              :elevation="hover ? 12 : 2"
              :class="{ 'on-hover': hover }"
            >
              <div
                v-ripple="{ class: `${card.color}--text` }"
                class="elevation-2"
              >
                <v-img
                  :src="require(`../assets/${card.src}`)"
                  class="white--text align-end pointer"
                  gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                  height="200px"
                >
                  <v-card-title v-text="card.title"></v-card-title>
                </v-img>
              </div>
            </v-card>
          </v-hover>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>
<script>
export default {
  props: {
    question: {},
    nextStep: Function,
  },
  data: () => {
    return {
      msgs: [
        "AH PARA",
        "ERROU",
        "JAMAIS",
        "DÁ NÃO",
        "AGORA FOI QUE EU VI",
        "PARA SÔ",
        "NUNCA",
        "VIAJA NÃO",
        "É A OUTRA OPÇÃO",
        "TENTE OUTRA VEZ",
        "NEM FODENDO"
      ],
    };
  },
  methods: {
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
.pointer {
  cursor: pointer;
}

.v-card.card-question {
  transition: opacity 0.4s ease-in-out;
}

.v-card.card-question:not(.on-hover) {
  opacity: 0.8;
}
</style>
