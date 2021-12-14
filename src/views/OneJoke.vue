<template>
  <div>
    <h1 v-if="loading">Loading...</h1>
    <div v-else>
      {{jokeFromAPI}}
    </div>
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
  name: "OneJoke",
  components: {},
  data: () => ({
    loading: true,
    jokeFromAPI: {} as JokesAPI,
  }),
  created() {
    const routerId = this.$route.params.id
    axios
      .get(`https://v2.jokeapi.dev/joke/Any?type=single&idRange=${routerId}`)
      .then(({ data }) => {
        this.jokeFromAPI = data.joke;
        this.loading = false;
        console.log(data.jokes);
      });
  },
});

</script>
