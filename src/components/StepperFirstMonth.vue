<template>
  <div>
    <v-stepper v-model="e1">
      <v-stepper-header>
        <template v-for="n in questions.length">
          <v-stepper-step :key="`${n}-step`" :complete="e1 > n" :step="n">
            Pergunta {{ n }}
          </v-stepper-step>

          <v-divider v-if="n !== questions.length" :key="n"></v-divider>
        </template>
      </v-stepper-header>

      <v-stepper-items>
        <v-stepper-content
          v-for="n in questions.length"
          :key="`${n}-content`"
          :step="n"
        >
          <v-card class="mb-12">
            <Question
              :question="questions[e1 - 1]"
              v-on:select-answer="selectedAnswer($event, n)"
              v-on:wrong-answer="wrongAnswer"
            />
          </v-card>
        </v-stepper-content>
      </v-stepper-items>
    </v-stepper>
  </div>
</template>

<script>
import Question from "./Question.vue";
const illustrations = require.context("@/assets", true, /^.*\.jpg$/);
export default {
  components: { Question },
  name: "StepperFirstMonth",
  data() {
    return {
      questions: [],
      e1: 1,
      steps: 2,
    };
  },

  watch: {
    steps(val) {
      if (this.e1 > val) {
        this.e1 = val;
      }
    },
  },

  created() {
    this.questions =[...this.getBabacas(), ...this.getNomeFilho()];
  },

  methods: {

    getBabacas() {
      return illustrations
      .keys()
      .filter((a) => a.includes("babacas"))
      .map((b) => {
        const jpg = b.replace("./", "");
        const nome = jpg.replace(".jpg", "").replace('babacas/','');
        return {
          question: "quem será seu marido?",
          cards: [
            {
              title: "Gustavin",
              src: "eu/eu.jpg",
              flex: 6,
              color: "success",
            },
            {
              title: nome.charAt(0).toUpperCase() + nome.slice(1),
              src: jpg,
              flex: 6,
              color: "error",
            },
          ],
        };
      });
    },

    getNomeFilho() {
      return [{
          question: "pra terminar, nome do nosso filho?",
          cards: [
            {
              title: "Gohan Militão",
              src: "eu/gohan.jpg",
              flex: 6,
              color: "success",
            },
            {
              title: "Noah Militão",
              src: "eu/noah.jpg",
              flex: 6,
              color: "error",
            },
          ],
        }];
    },

    nextStep(n) {
      if (n === this.questions.length) {
        this.$emit("finish");
        console.log(this.questions);
      } else {
        this.e1 = n + 1;
      }
    },

    wrongAnswer(msg) {
      this.$emit("wrong-answer", msg);
    },

    selectedAnswer(card, n) {
      this.questions[n - 1].answer = card;
      this.nextStep(n);
    },

    previousStep(n) {
      if (n === 1) {
        this.e1 = 1;
      } else {
        this.e1 = n - 1;
      }
    },
  },
};
</script>
