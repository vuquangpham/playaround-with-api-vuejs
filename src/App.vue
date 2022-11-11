<template>
  <LearningSurvey @survey-submit="handlePostData" />
  <UserExperiences :results="results" />
</template>

<script>
import LearningSurvey from "./components/survey/LearningSurvey.vue";
import UserExperiences from "./components/survey/UserExperiences.vue";

export default {
  components: {
    LearningSurvey,
    UserExperiences,
  },
  data() {
    return {
      results: [],
      isLoading: false,
    };
  },
  provide() {
    return {
      isLoading: this.isLoading,
    };
  },
  methods: {
    handlePostData(data) {
      fetch(
        "https://vuejs-demo-sending-http-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(data),
        }
      )
        .then((response) => {
          if (!response.ok) {
            throw new Error(`Can't POST data to databse`);
          }
          this.results.push(data);
        })
        .catch((err) => alert(err.message));
    },
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
              id: data[id].id,
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

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>
