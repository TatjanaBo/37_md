<template>
  <h1 v-if="loading">Loading...</h1>
  <div>
    <span>{{jokeInfo.id}}</span>
    <p>{{jokeInfo.question}}</p>
   <p>{{jokeInfo.answer}}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Joke, STORAGE_KEY } from "@/views/Home.vue";

export default defineComponent({
  name: "CreatedJoke",
  components:{},
  data:() => ({
    loading:true,
    jokeInfo: JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"),
  }),
});

</script>
