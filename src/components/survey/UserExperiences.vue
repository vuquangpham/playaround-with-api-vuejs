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
  data() {
    return {
      results: [],
      isEmpty: false,
      isLoading: false,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      // GET data from database
      fetch(
        "https://vuejs-demo-sending-http-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json"
      )
        .then((response) => {
          if (!response.ok) throw new Error(`Can't get data from API!!!!`);
          return response.json();
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
              experience: data[id].experience || "No thing",
            });
          }
          this.results = results;
          if (this.results.length === 0) this.isEmpty = true;
          else this.isEmpty = false;
        })
        .catch((err) => {
          alert(err.message);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
  },
  mounted() {
    this.loadExperiences();
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
