<template>
  <div id="sidebar">
    <img id="logo" alt="logo" src="../assets/fluffy-math.png" width="60px" height="60px">
    <div id="title">Fluffy Math</div>
    <div id="sidebar-description">生成你的四则运算题目</div>

    <b-form id="choices" @submit="onSubmit" v-if="show">
      <b-form-group id="difficulty" label="题目难度" label-for="difficulty-choice">
        <b-form-select
          id="difficulty-choice"
          v-model="form.difficulty"
          :options="difficulties"
          class="mb-3"
        />
      </b-form-group>
      <b-form-group id="quantity" label="题目数量" label-for="quantity-choice">
        <b-form-input
          id="quantity-choice"
          v-model="form.quantity"
          :options="difficulties"
          class="mb-3"
        />
      </b-form-group>
      <b-form-group id="type" label="题目种类" label-for="type-choice">
        <b-form-select id="type-choice" v-model="form.type" :options="types" class="mb-3"/>
      </b-form-group>
      <b-form-group id="powerIndicator" label="乘方表示" label-for="power-choice">
        <b-form-select
          id="power-choice"
          v-model="form.powerIndicator"
          :options="powers"
          class="mb-3"
        />
      </b-form-group>
      <b-button id="submit-btn" type="submit" class="btn btn-warning btn-lg btn-block">生成题目</b-button>
    </b-form>

    <div id="footer">
      <p>
        Created with Vue, Bootstrap and love <i class="fa fa-heart" aria-hidden="true"></i>.
      </p>
      <p>
        Open source on <a href="https://github.com/spencerwooo/fluffy-math-spa"><i class="fa fa-github" aria-hidden="true"></i> GitHub</a>.
      </p>
      <p>Copyright © 2019 Fluffy Math Team</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Sidebar",
  data() {
    return {
      form: {
        difficulty: "easy",
        quantity: 15,
        type: "integer",
        powerIndicator: "caret",
        problemSet: []
      },
      difficulties: [
        { value: "easy", text: "小学一年级难度" },
        { value: "hard", text: "小学二年级难度" },
        { value: "hell", text: "小学三年级难度" }
      ],
      types: [
        { value: "integer", text: "整数四则运算" },
        { value: "fractional", text: "分数四则运算" }
      ],
      powers: [
        { value: "caret", text: "^" },
        { value: "asterisk", text: "**" }
      ],
      show: true
    };
  },
  methods: {
    getRandomNumber() {
      return Math.floor(Math.random() * 101);
    },
    getSmallRandomNumber() {
      return Math.floor(Math.random() * 10);
    },
    getRandomOperator(type) {
      let operators = [];
      if (type === "caret") {
        operators = ["+", "-", "*", "÷", "^"];
      } else {
        operators = ["+", "-", "*", "÷", "**"];
      }
      let randomoperator = Math.random();
      if (randomoperator > 0.7) {
        return operators[0];
      } else if (randomoperator > 0.4) {
        return operators[1];
      } else if (randomoperator > 0.15) {
        return operators[2];
      } else if (randomoperator > 0.05) {
        return operators[3];
      } else {
        return operators[4];
      }
    },
    generateIntegerProblem(problemNum, powerType) {
      let problemList = [];
      let bracketflag = 0; // 能不能加右括号
      let numflag = 0; // 缺少的右括号个数
      let block = 0; // 避免（3）这种情况
      let tmp = ""; // 存储运算符，进行运算类型判断，以约束运算数
      while (problemNum--) {
        bracketflag = 0;
        numflag = 0;
        block = 0;
        let end = 0;
        let start = 0;
        let Plen = Math.floor(Math.random() * 10) + 1;
        let problem = this.getRandomNumber();
        while (Plen--) {
          block = 0;
          problem = problem + ' '
          tmp = this.getRandomOperator(powerType);
          problem = problem + tmp;
          if (tmp === "**" || tmp === "^") {
            end = problem.length;
            start = end - 3;
            while (start) {
               if (start === 1) {
              problem = problem.substring(0, start) + ' '
              problem = problem + tmp
              break
            }
              if (
                (problem[start] >= "0" && problem[start] <= "9") ||
                problem[start] === ")" || problem[start] === ' '
              ) {
                if (problem[start] === ')') {
                numflag++
              }
                problem = problem.substring(0, start + 2) + ' ';
                problem = problem + tmp;
                start--;
              } else break;
            }
            problem = problem + ' '
            problem += this.getSmallRandomNumber();
          } else {
            problem = problem + ' '
            if (Plen > 1) {
              if (Math.random() > 0.8) {
                problem += "(";
                bracketflag = 1;
                numflag++;
                block = 1;
              }
            }
            problem += this.getRandomNumber();
          }
          if (bracketflag) {
            if (Math.random() > 0.7) {
              if (!block) {
                problem += ")";
                bracketflag = 0;
                numflag -= 1;
              }
            }
          }
        }
        while (numflag > 0) {
          problem += ")";
          numflag--;
        }
        problemList.push(problem);
      }
      return problemList;
    },
    onSubmit(evt) {
      evt.preventDefault();
      if (this.form.quantity > 20) {
        alert("Quantity is too large");
        this.form.quantity = 15;
      } else {
        this.form.problemSet = this.generateIntegerProblem(
          this.form.quantity,
          this.form.powerIndicator
        );
        this.$emit("passDataToApp", this.form);
      }
    }
  }
};
</script>

<style scoped>
#sidebar {
  background: #2176ff;
  background: -moz-linear-gradient(
    45deg,
    #3d28a8 24%,
    #3d28a8 24%,
    #2176ff 99%
  );
  background: -webkit-linear-gradient(
    45deg,
    #3d28a8 24%,
    #3d28a8 24%,
    #2176ff 99%
  );
  background: linear-gradient(45deg, #3d28a8 24%, #3d28a8 24%, #2176ff 99%);
  width: 24rem;
  position: fixed;
  z-index: 10;
  margin: 0;
  top: 0;
  left: 0;
  bottom: 0;
  text-align: center;
  padding: 2rem;
  overflow: auto;
  -webkit-box-shadow: inset -7px 0 19px -7px rgba(0, 0, 0, 0.4);
  -moz-box-shadow: inset -7px 0 19px -7px rgba(0, 0, 0, 0.4);
  box-shadow: inset -7px 0 19px -7px rgba(0, 0, 0, 0.4);
}

a {
  color: #FDCA40;
}

a:hover {
  color: #FDCA40;
  text-decoration-style: solid;
}

#logo {
  margin: 1rem 0;
}

#title {
  margin: 1rem 0;
  font-size: 36px;
  font-weight: 600;
  line-height: 1.25;
  display: block;
  color: white;
}

#sidebar-description {
  margin: 1rem 0;
  font-size: 18px;
  color: white;
  font-weight: 400;
}

#choices {
  margin-top: 4rem;
  text-align: left;
  color: white;
}

#submit-btn {
  color: white;
  font-weight: 600;
}

#footer {
  position: fixed;
  bottom: 0;
  text-align: left;
  color: white;
  font-size: 12px;
  font-weight: 200;
  line-height: 0.5;
  margin-bottom: 1rem;
}
</style>
