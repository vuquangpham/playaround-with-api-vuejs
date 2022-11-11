<template>
  <section>
    <BaseCard>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input
            type="radio"
            id="rating-poor"
            value="poor"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <div class="form-control">
          <label for="experience">Your experience:</label>
          <textarea id="experience" rows="5" ref="experience"></textarea>
        </div>
        <p
          v-if="invalidInput"
          :style="{
            color: 'red',
          }"
        >
          One or more input fields are invalid. Please check your provided data.
        </p>
        <div>
          <BaseButton>Submit</BaseButton>
        </div>
      </form>
    </BaseCard>
  </section>
</template>

<script>
import BaseButton from "../UI/BaseButton.vue";
import BaseCard from "../UI/BaseCard.vue";

export default {
  components: { BaseButton, BaseCard },
  data() {
    return {
      enteredName: "",
      chosenRating: null,
      invalidInput: false,
    };
  },
  emits: ["survey-submit"],
  methods: {
    submitSurvey() {
      if (this.enteredName === "" || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      const data = {
        id: new Date().toISOString(),
        name: this.enteredName,
        rating: this.chosenRating,
        experience: this.$refs.experience.value.trim(),
      };

      this.$emit("survey-submit", data);

      this.enteredName = "";
      this.$refs.experience.value = "";
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 1rem 0;
}

input[type="text"] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}

label {
  font-weight: bold;
}

textarea {
  display: block;
  width: 20rem;
}
</style>
