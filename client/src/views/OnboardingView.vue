<template>
  <div class="page-wrapper">
    <div class="container">
      <div class="onboardin-popover" v-if="isOnboarding">
        <h3 class="question">{{ title }}</h3>
        <p class="description" v-if="description?.length > 0">{{ description }}</p>
        <div class="onboarding-answers">
          <button class="answer-button" v-for="answer in answers" @click="selectResponse(answer.id)" :class="{ '--selected': providedAnswers.includes(answer.id) }">{{ answer.text }}</button>
          <input type="text" v-if="questionType === 3" placeholder="Your text" />
        </div>

        <div class="onboarding-controls">
          <button @click="submitResponse" class="continue-button">Next</button>
          <p @click="selectResponse(null)" class="onboarding-skip-button">I know what I want, skip</p>

        </div>


      </div>

      <div v-else>
        <h2>You're all set!</h2>
        <p>You will be redirected shortly.</p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from 'pinia';
import { useOnboardingStore } from '../stores/onboarding';

// User should not be able to open this page while not in onboarding
export default {
  computed: {
    ...mapState(useOnboardingStore, { answers: 'answers', description: 'description', isOnboarding: 'isOnboarding', providedAnswers: 'providedAnswers', title: 'question', questionType: 'type' }),
  },
  methods: {
    ...mapActions(useOnboardingStore, { selectResponse: 'selectResponse', submitResponse: 'submitResponse' }),
  },
};
</script>

<style scoped>
.page-wrapper {
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: 100vh;
}

.container {
  background-color: #fff;
  padding: 30px;
  box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px;
}

.onboarding-answers {
  display: flex;
  gap: 8px;
  justify-content: space-evenly;
  padding: 26px 0 18px;
}
.answer-button {
  border: 2px solid rgb(0, 72, 255);
  border-radius: 18px;
  color: rgb(0, 72, 255);
  cursor: pointer;
  font-weight: 500;
  padding: 10px;
}

.answer-button.--selected {
  background-color: rgb(0, 72, 255);
  color: #fff;
}

.description {
  font-size: 15px;
}

.onboarding-controls {
  display: flex;
  gap: 22px;
}

.continue-button {
  background-color: rgb(0, 72, 255);;
  border: none;
  border-radius: 4px;
  color: #fff;
  font-size: 14px;
  padding: 12px 24px;
}

.onboarding-skip-button {
  align-self: center;
  color: #333;
  cursor: pointer;
  font-size: 14px;
  text-align: center;
  text-decoration: underline;
}

</style>
