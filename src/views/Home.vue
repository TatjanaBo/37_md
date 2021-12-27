<template>
  <div class="home">
    <h2>Create a joke</h2>
    <div class="enter-joke">
      <p class="heading">Question</p>
      <input
        class="input-field"
        type="text"
        v-model="questionValue"
        placeholder="question"
      />
      <p class="heading">Answer</p>
      <input
        class="input-field"
        type="text"
        v-model="answerValue"
        placeholder="answer"
      />
      <button class="add-btn" @click="addJoke">Create</button>
      <input
        class="input-field"
        type="text"
        v-model="searchValue"
        placeholder="search"
      />
    </div>
    <div class="jokes-wrapper">
      <div class="one-joke" v-for="(item, index) in filterJokes()" :key="item">
        <router-link class="joke-link" :to="`/jokes/${item.id}`">
          <div class="question-answer">{{ item.question }}</div>
        </router-link>
        <transition name="answer">
          <div v-if="showAnswer">
            <div class="question-answer">{{ item.answer }}</div>
            <div class="time">{{ item.localTime }}</div>
          </div>
        </transition>
        <div class="button-wrapper">
          <button @click="toggleJokes(index)" class="joke-action-btn">
            {{ showAnswer ? "Hide Answer" : "Tell Me" }}
          </button>
          <button class="joke-action-btn" @click="deleteJoke(index)">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export type Joke = {
  question: string;
  answer: string;
  id: number;
  localTime: string;
};

export const STORAGE_KEY = "joke-storage";

export default defineComponent({
  name: "Home",
  components: {},
  data: () => ({
    questionValue: "",
    answerValue: "",
    jokes: [
      {
        question: "How do you make holy water?",
        answer: "I do not know",
        id: 1,
        localTime: String(new Date()),
      } as Joke,
    ],
    searchValue: "",
    showAnswer: false,
  }),
  created() {
    this.jokes = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
  },
  methods: {
    addJoke() {
      this.jokes.push({
        question: this.questionValue,
        answer: this.answerValue,
        id: this.jokes.length,
        localTime: String(new Date()),
      });
      this.questionValue = "";
      this.answerValue = "";
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.jokes));
    },
    deleteJoke(i: number) {
      const newJokesList = this.jokes.filter((item, index) => index !== i);
      this.jokes = newJokesList;
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.jokes));
    },
    filterJokes() {
      return this.jokes.filter((item) =>
        item.question.toLowerCase().includes(this.searchValue.toLowerCase())
      );
    },
    toggleJokes(i: number) {
      const newJokesList = this.jokes.filter((item, index) => index === i);
      this.showAnswer = !this.showAnswer;
    },
  },
});
</script>

<style lang="scss">
body {
  background-color: rgba(0, 0, 0, 0.5);
}
.home {
  display: grid;
  justify-content: center;
}
.enter-joke {
  max-width: 440px;
  display: grid;
  justify-content: left;
  gap: 15px;
  background-color: #379877;
  color: white;
  border-radius: 5px;
  padding: 20px;
  margin-bottom: 10px;
  box-shadow: 6px 6px 20px;
}

.joke-link {
  text-decoration: none;
}

.question-answer {
  text-align: left;
}

.heading {
  margin-bottom: 0;
  margin-top: 0;
  text-align: left;

  &:first-child {
    margin-top: 15px;
  }
}

.input-field {
  width: 400px;
  height: 20px;

  &:last-child {
    width: 200px;
  }
}

.add-btn {
  padding: 10px 20px;
  background-color: #154131;
  color: white;
  cursor: pointer;
  border: none;
  border-radius: 4px;
  width: 80px;

  &:hover {
    background-color: #42b983;
    transition: 0.5s;
  }
}

.jokes-wrapper {
  display: grid;
  gap: 15px;
  justify-content: center;
}

.one-joke {
  width: 420px;
  display: grid;
  justify-content: left;
  gad: 10px;
  border-radius: 5px;
  padding: 10px;
  background-color: white;
  box-shadow: 6px 6px 20px;
}

.time {
  font-size: 10px;
}

.button-wrapper {
  display: flex;
  gap: 5px;
}

.joke-action-btn {
  padding: 10px 20px;
  cursor: pointer;
  color: white;
  background-color: #e85252;
  border-radius: 4px;
  border: none;

  &:first-child {
    background-color: #d7d770;
    color: #3b3131;
  }
  &:hover {
    opacity: 0.5;
  }
}

.answer-enter-active,
.answer-leave-active{
  transition: opacity 0.8s ease;
}

.answer-enter-from,
.answer-leave-to {
  opacity: 0;
}
</style>
