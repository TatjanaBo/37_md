<template>
  <div class="api-jokes">

    <h1>Random jokes</h1>
    <input type="text"
    placeholder="search"
    v-model="searchValue">
    <h1 v-if="loading">Loading...</h1>
    <template v-else class="jokes-container">
      <div v-for="joke in filterRandomJokes()" :key="joke.id">
        <router-link class="joke-link" :to="`/random-jokes/${joke.id}`">
        <p class="joke-background">{{ joke.joke }}</p>
        </router-link>
      </div>
    </template>

  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

type JokesAPI = {
  category: string;
  type: string;
  joke: string;
  id: number;
};

export default defineComponent({
  name: " ",
  components: {},
  data: () => ({
    loading: true,
    jokesFromAPI: [] as JokesAPI[],
    searchValue: '',
  }),
  created() {
    axios
      .get("https://v2.jokeapi.dev/joke/Programming?type=single&amount=10")
      .then(({ data }) => {
        this.jokesFromAPI = data.jokes;
        this.loading = false;
        console.log(data.jokes);
      });
  },
  methods:{
    filterRandomJokes(){
      return this.jokesFromAPI.filter((item) => item.joke.toLowerCase().includes(this.searchValue.toLowerCase()))
    }
  },
});
</script>

<style lang="scss">
*{
  box-sizing: border-box;
}

.api-jokes {
  padding: 10px;
}

.jokes-container{
  display: grid;
  //grid-template-columns: repeat(auto-fit, 200px);
  gap: 15px;
  //justify-content: center;
}

.joke-background{
  background-color: #42b983;
  border-radius: 5px;
}
.joke-link{
  text-decoration: none;
  color:inherit;
}
</style>
