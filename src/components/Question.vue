<template>
  <b-container fluid>
    <b-row class="d-flex justify-content-center align-items-center">
      <h5 class="text-center">{{ number }}. {{ text }}</h5>
    </b-row>

    <!-- wrapper div to control if 2 or 4 options -->
    <div v-if="options.length == 2">
      <b-row class="quarter d-flex" v-if="options.length == 2">
        <b-col
          v-for="option in options"
          v-bind:key="option.id"
          class="d-flex justify-content-center align-items-center"
        >
          <b-button pill @click="$emit('increase', option)" variant="primary">
            {{ option }}
          </b-button>
        </b-col>
      </b-row>
    </div>
    <div v-else-if="options.length == 4">
      <b-row class="quarter d-flex">
        <b-col
          v-for="option in options.slice(0, 2)"
          v-bind:key="option.id"
          class="d-flex justify-content-center align-items-center"
        >
          <b-button pill @click="$emit('increase', option)" variant="primary">
            {{ option }}
          </b-button>
        </b-col>
      </b-row>
      <b-row class="quarter d-flex">
        <b-col
          v-for="option in options.slice(2, 4)"
          v-bind:key="option.id"
          class="d-flex justify-content-center align-items-center"
        >
          <b-button pill @click="$emit('increase', option)" variant="primary">
            {{ option }}
          </b-button>
        </b-col>
      </b-row>
    </div>
  </b-container>
</template>

<script>
export default {
  name: "Question",
  props: ["number", "text", "options"],
};
</script>

<style scoped>
.quarter {
  height: 20vh;
}

button {
  padding: 25px;
}
</style>