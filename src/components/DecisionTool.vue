<template>
  <b-container fluid id="tool" class="pt-5">
    <!-- if questions remain, ask them -->
    <div v-if="current_question">
      <p
        v-if="this.questions[this.current_question].warning"
        class="text-center text-warning"
      >
        {{ this.questions[this.current_question].warning }}
      </p>
      <!-- Question component -->
      <question v-bind="selectedQuestion" v-on:increase="handleAnswer" />

      <div v-if="current_question != 'track'">
        <!-- Back Button -->
        <b-row class="quarter d-flex justify-content-center">
          <b-col
            cols="6"
            class="d-flex justify-content-center align-items-center"
          >
            <b-button
              pill
              block
              variant="primary"
              @click="current_question = 'track'"
            >
              <b-icon icon="skip-backward-fill"></b-icon>
            </b-button>
          </b-col>
        </b-row>
      </div>
    </div>
    <!-- otherwise show the results -->
    <div v-else><result :answers="chosen_answers" /></div>
  </b-container>
</template>

<script>
import Question from "./Question.vue";
import Result from "./Result.vue";

export default {
  components: { Question, Result },
  name: "DecisionTool",
  data() {
    return {
      current_question: "track",
      chosen_answers: {},
      questions: {
        track: {
          number: 1,
          text: "What best describes the patient's disease state?",
          options: ["LTRI low/moderate-acuity", "LTRI high-acuity or Sepsis"],
        },
        low_acuity: {
          number: 2, // low-moderate acuity
          text:
            "What range does the current procalictonin level fall in? (ng/ml)",
          options: [
            "<0.10 or drop by >90%",
            "0.10 - 0.24 or drop by >80%",
            "0.25 - 0.50",
            ">0.50",
          ],
        },
        high_acuity: {
          number: 2, // high acuity
          text:
            "What range does the current procalictonin level fall in? (ng/ml)",
          options: [
            "<0.25 or drop by >90%",
            "0.25 - 0.49 or drop by >80%",
            "0.50 - 0.99",
            ">1.00",
          ],
          warning:
            "**Algorithm only recommended on Day 2+ for Sepsis patients.***",
        },
      },
    };
  },
  methods: {
    handleAnswer: function (answer) {
      this.chosen_answers[this.current_question] = answer;

      if (answer === "LTRI high-acuity or Sepsis") {
        this.current_question = "high_acuity";
      } else if (answer === "LTRI low/moderate-acuity") {
        this.current_question = "low_acuity";
      } else {
        this.current_question = null;
      }
    },
  },
  computed: {
    selectedQuestion: function () {
      return this.questions[this.current_question];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#tool {
  background-image: linear-gradient(white 50%, blue);
  height: 100vh;
}
.quarter {
  height: 25vh;
}
</style>
