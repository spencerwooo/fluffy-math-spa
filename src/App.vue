<template>
  <div id="app">
    <Sidebar v-on:passDataToApp="onProblemSubmit"/>
    <div id="page">
      <div id="title">Fluffy Math 出题优</div>

      <div id="description">
        生成 <strong>{{ problem.difficulty }}</strong> 难度的，<strong>{{ problem.type }}</strong> 类型，共 {{ problem.quantity }} 道题目。
        乘方用 {{ problem.powerIndicator }} 表示。
      </div>

      <div id="problem">生成的题目</div>
      <div id="problem-container">
        <ul>
          <li v-for="problems in generatedProblems" :key="problems">{{ problems }}</li>
        </ul>
      </div>
      <img id="corner-image" src="./assets/corner-image.jpg" alt="People doing math" width="25%">
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
      generating: true,
      problem: {
        difficulty: "小学一年级难度",
        quantity: 15,
        type: "整数四则运算",
        powerIndicator: "^",
        problemSet: []
      },
      generatedProblems: ["1 + 1", "1 / 3 + 2 / 5"]
    };
  },
  methods: {
    onProblemSubmit(problem) {
      this.generatedProblems = problem.problemSet;
      let configMap = {
        'easy': '小学一年级难度',
        'hard': '小学二年级难度',
        'hell': '小学三年级难度',
        'integer': '整数四则运算',
        'fractional': '分数四则运算',
        'caret': '^',
        'asterisk': '**'
      };
      this.problem.difficulty = configMap[problem.difficulty];
      this.problem.type = configMap[problem.type];
      this.problem.powerIndicator = configMap[problem.powerIndicator];
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
  font-weight: bold;
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
  font-weight: bold;
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
#corner-image {
  position: fixed;
  margin: 3rem;
  bottom: 0;
  right: 0;
}
</style>
