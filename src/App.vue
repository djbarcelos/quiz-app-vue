<template>
  <div class="container">
    <div class="title">Super Quiz</div>
  </div>
  <QuizApp />
</template>

<script>
import QuizApp from "./components/Quiz-App.vue";

export default {
  components: {
    QuizApp,
  },
};
</script>

<style>
body {
  font-family: "Lato", sans-serif;
  background: linear-gradient(to right, #8de4f7, #008de3);
  color: #fff;
}

#app {
  font-family: "Big Shoulders Display", cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
  font-size: 2rem;
  display: flex;
  flex-direction: column;
}

.title {
  font-size: 4rem;
  font-weight: 600;
}
</style>
