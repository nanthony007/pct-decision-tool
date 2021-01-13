<template>
  <b-container fluid id="tool" class="pt-5">
    <b-row class="text-center justify-content-center">
      <b-col cols="6">
        <h3>
          <small>Result:</small>
          <br />
          <span v-if="processedAnswers.color == 'danger'">
            <span class="text-danger"
              >Antibiotics {{ processedAnswers.result }}</span
            >
          </span>
          <span v-if="processedAnswers.color == 'warning'">
            <span class="text-warning"
              >Antibiotics {{ processedAnswers.result }}</span
            >
          </span>
          <span v-if="processedAnswers.color == 'info'">
            <span class="text-info"
              >Antibiotics {{ processedAnswers.result }}</span
            >
          </span>
          <span v-if="processedAnswers.color == 'success'">
            <span class="text-success"
              >Antibiotics {{ processedAnswers.result }}</span
            >
          </span>
        </h3>
        <ul
          class="text-left"
          v-for="msg in processedAnswers.messages"
          :key="msg.index"
        >
          <li>{{ msg }}</li>
        </ul></b-col
      >
    </b-row>

    <b-row>
      <b-col cols="6">
        <h3 class="text-center text-warning">
          {{ caution.title.toUpperCase() }}
        </h3>
        <h5 class="text-center">{{ caution.header }}</h5>
        <ul
          class="py-0 px-2 my-0 mx-2"
          v-for="bullet in caution.bullets"
          :key="bullet.index"
        >
          <li>{{ bullet }}</li>
        </ul>
      </b-col>
      <b-col cols="6">
        <h3 class="text-center text-danger">
          {{ warning.title.toUpperCase() }}
        </h3>
        <h5 class="text-center">{{ warning.header }}</h5>
        <ul>
          <li>{{ warning.header2 }}</li>
          <ul
            class="py-0 px-2 my-0 mx-2"
            v-for="bullet in warning.bullets"
            :key="bullet.index"
          >
            <li>{{ bullet }}</li>
          </ul>
        </ul>
      </b-col>
    </b-row>
    <!-- Back Button -->
    <b-row class="quarter d-flex justify-content-center">
      <b-col cols="6" class="d-flex justify-content-center align-items-center">
        <b-button pill block variant="primary" @click="reloadPage()">
          <b-icon icon="arrow-clockwise"></b-icon> RESTART
        </b-button>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "Result",
  props: ["answers"],
  data() {
    return {
      possibleResults: {
        high_acuity: {
          firstRange: {
            result: "Strongly Discouraged",
            color: "danger",
            messages: [
              "If clinically unstable, consider overruling recommendation.",
            ],
          },
          secondRange: {
            result: "Discouraged",
            color: "warning",
            messages: [
              "If clinically unstable, consider overruling recommendation.",
            ],
          },
          thirdRange: {
            result: "Encouraged",
            color: "info",
            messages: [
              "Repeat test every 2-3 days and use algorithm to evaluate discontinuation.",
              "If PCT rising or not decreasing, consider treatment failure. Consider expanding antibiotic coverage and further evaluation.",
            ],
          },
          fourthRange: {
            result: "Strongly Encouraged",
            color: "success",
            messages: [
              "Repeat test every 2-3 days and use algorithm to evaluate discontinuation.",
              "If PCT rising or not decreasing, consider treatment failure. Consider expanding antibiotic coverage and further evaluation.",
            ],
          },
        },
        low_acuity: {
          firstRange: {
            result: "Strongly Discouraged",
            color: "danger",
            messages: [
              "Consider alternate diagnosis.",
              "Repeat PCT in 6-24 hours if no clinical imporvement.",
              "If immunosupressed or high risk consider overrule.",
            ],
          },
          secondRange: {
            result: "Discouraged",
            color: "warning",
            messages: [
              "Consider alternate diagnosis.",
              "Repeat PCT in 6-24 hours if no clinical imporvement.",
              "If immunosupressed or high risk consider overrule.",
            ],
          },
          thirdRange: {
            result: "Encouraged",
            color: "info",
            messages: [
              "Repeat test every 2-3 days and use algorithm to evaluate discontinuation.",
              "If PCT rising or not decreasing, consider treatment failure. Consider expanding antibiotic coverage and further evaluation.",
            ],
          },
          fourthRange: {
            result: "Strongly Encouraged",
            color: "success",
            messages: [
              "Repeat test every 2-3 days and use algorithm to evaluate discontinuation.",
              "If PCT rising or not decreasing, consider treatment failure. Consider expanding antibiotic coverage and further evaluation.",
            ],
          },
        },
      },
      caution: {
        title: "Slow",
        header: "Non-bacterial causes of PCT elevation",
        bullets: [
          "Massive stress",
          "Malaria",
          "Systemic vasculitis",
          "Agents that stimulate cytokines",
          "End-stage renal disease",
          "Immunocompromised",
        ],
      },
      warning: {
        title: "Stop",
        header: "Do NOT use PCT for these patients",
        header2: "Chronic infections (including but not limited to):",
        bullets: ["Osteomyelitis", "Abscess", "Subacute endocarditis"],
      },
    };
  },
  methods: {
    reloadPage() {
      window.location.reload();
    },
  },
  computed: {
    processedAnswers: function () {
      var stringTrack = this.answers.track;
      var stringRange;
      var selectedTrack = "";
      var selectedRange = "";

      // lots of conditional logic

      if (stringTrack === "LRTI high-acuity or Sepsis") {
        selectedTrack = "high_acuity";
        stringRange = this.answers.high_acuity;
        if (stringRange === "<0.25 or drop by >90%") {
          selectedRange = "firstRange";
        } else if (stringRange === "0.25 - 0.49 or drop by >80%") {
          selectedRange = "secondRange";
        } else if (stringRange === "0.50 - 0.99") {
          selectedRange = "thirdRange";
        } else if (stringRange === ">1.00") {
          selectedRange = "fourthRange";
        }
      } else if (stringTrack === "LRTI low/moderate-acuity") {
        selectedTrack = "low_acuity";
        stringRange = this.answers.low_acuity;
        if (stringRange === "<0.10 or drop by >90%") {
          selectedRange = "firstRange";
        } else if (stringRange === "0.10 - 0.24 or drop by >80%") {
          selectedRange = "secondRange";
        } else if (stringRange === "0.25 - 0.50") {
          selectedRange = "thirdRange";
        } else if (stringRange === ">0.50") {
          selectedRange = "fourthRange";
        }
      } else {
        console.log("BADDD");
      }
      console.log(selectedTrack);
      console.log(selectedRange);
      // access result from possibleResults using conditional logic
      return this.possibleResults[selectedTrack][selectedRange];
    },
  },
};
</script>

<style scoped></style>