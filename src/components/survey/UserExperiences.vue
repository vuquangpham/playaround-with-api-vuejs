<template>
  <section>
    <BaseCard>
      <h2>Submitted Experiences</h2>
      <p v-if="isLoading">Loading......</p>
      <ul>
        <SurveyResult
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
          :experience="result.experience"
        />
      </ul>
      <p v-if="isEmpty">No experiences before!!!</p>
    </BaseCard>
  </section>
</template>

<script>
import BaseCard from "../UI/BaseCard.vue";
import SurveyResult from "./SurveyResult.vue";

export default {
  components: {
    SurveyResult,
    BaseCard,
  },
  props: ["results"],
  inject: ["isLoading"],
  data() {
    return {
      isEmpty: false,
    };
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
