<template>
  <div id="app">
    <Sidebar v-on:passDataToApp="onProblemSubmit"/>
    <div id="page">
      <div id="title">Fluffy Math 出题优</div>

      <div id="description">
        生成 {{ problem.difficulty }} 难度的，{{ problem.type }} 类型，共 {{ problem.quantity }} 道题目。
        乘方用 {{ problem.powerIndicator }} 表示。
      </div>

      <div id="problem">生成的题目</div>
      <div id="problem-container">
        <ul>
          <li v-for="problems in generatedProblems" :key="problems">{{ problems }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from "./components/Sidebar.vue";

export default {
  name: "app",
  components: {
    Sidebar
  },
  data() {
    return {
      problem: {
        difficulty: "easy",
        quantity: 15,
        type: "integer",
        powerIndicator: "caret",
        problemSet: []
      },
      generatedProblems: []
    };
  },
  methods: {
    onProblemSubmit(problem) {
      this.generatedProblems = problem.problemSet;
      this.problem = problem;
    }
  }
};
</script>

<style>
#app {
  font-family: "Noto Sans SC", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-left: 30rem;
  color: #2c3e50;
}
#page {
  margin-top: 5rem;
}
#title {
  margin: 1rem 0;
  font-size: 36px;
  font-weight: 600;
  line-height: 1.25;
  display: block;
}
#description {
  margin: 40px 0;
  padding-left: 1rem;
  border-left: #2176ff solid 5px;
  color: #808080;
}
#problem {
  margin: 1rem 0;
  font-size: 24px;
  font-weight: 600;
  line-height: 1.25;
}
#problem-container {
  margin-bottom: 5rem;
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 30px 0;
}
li {
  margin: 10px 0;
}
a {
  color: #2176ff;
}
</style>
