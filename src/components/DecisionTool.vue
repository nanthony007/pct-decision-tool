<template>
  <b-container fluid id="tool" class="pt-5">
    <!-- if questions remain, ask them -->
    <div v-if="current_question < question_list.length">
      <!-- Question component -->
      <question v-bind="selectedQuestion" v-on:increase="handleAnswer" />

      <div v-if="current_question != 0">
        <!-- Back Button -->
        <b-row class="quarter d-flex justify-content-center">
          <b-col
            cols="6"
            class="d-flex justify-content-center align-items-center"
          >
            <b-button pill variant="primary" @click="current_question -= 1">
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
      current_question: 0,
      chosen_answers: [],
      question_list: [
        {
          number: 1,
          text: "What best describes the patient's disease state?",
          options: [
            "LTRI low-acuity",
            "LTRI high-acuity",
            "Sepsis low-acuity",
            "Sepsis high-acuity",
          ],
        },
        {
          number: 2,
          text: "Is this the first procalcitonin order for this patient?",
          options: ["Yes", "No"],
        },
        {
          number: 3,
          text:
            "What range does the current procalictonin level fall in? (ng/ml)",
          options: ["< 0.10", "0.10 - 0.24", "0.25 - 0.50", "> 0.50"],
        },
        {
          number: 4,
          text:
            "If not the first test, what range did the most recent previous procalcitonin level fall in? (ng/ml)",
          options: ["< 0.10", "0.10 - 0.24", "0.25 - 0.50", "> 0.50"],
        },
      ],
    };
  },
  methods: {
    handleAnswer: function (answer) {
      this.chosen_answers[this.current_question] = answer;
      this.current_question += 1;
    },
  },
  computed: {
    selectedQuestion: function () {
      // will need to add -1 here eventually when add landing/start page
      return this.question_list[this.current_question];
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
