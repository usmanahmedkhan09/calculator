<template>
  <b-container class="container">
    <div class="board p-2">
      <div class="d-flex justify-content-between">
        <img class="text-left" src="/history.png" alt="" />
        <span v-if="showExpression">
          {{ exp.join(" ").replace(/([+,*,/,-])/g, " $1 ") }} =
        </span>
        <span v-if="showAns"> Ans = {{ result }} </span>
      </div>
      <div class="text-end h3">
        {{ currentValue ? currentValue.replace(/([+,*,/,-])/g, " $1 ") : 0 }}
      </div>
    </div>
    <div class="buttons-container mt-2">
      <div class="sections">
        <div class="mb-1 custom-button-container">
          <button class="button">
            <span class="mx-4">Rad</span> |
            <span class="mx-4 text-muted">Deg</span>
          </button>
          <button class="button">x!</button>
        </div>
        <div class="grid mb-1">
          <button class="button">Inv</button>
          <button class="button">sin</button>
          <button class="button">In</button>
        </div>
        <div class="grid mb-1">
          <button class="button">&prod;</button>
          <button class="button">cos</button>
          <button class="button">log</button>
        </div>
        <div class="grid mb-1">
          <button class="button">e</button>
          <button class="button">tan</button>
          <button class="button">&radic;</button>
        </div>
        <div class="grid mb-1">
          <button class="button">Ans</button>
          <button class="button">EXP</button>
          <button class="button">x<sup>y</sup></button>
        </div>
      </div>
      <div class="sections">
        <div class="grid mb-1">
          <button class="button">(</button>
          <button class="button">)</button>
          <button class="button">%</button>
        </div>
        <div class="grid mb-1">
          <button
            @click="onNumberClick('7')"
            value="7"
            class="button button-light"
          >
            7
          </button>
          <button
            @click="onNumberClick('8')"
            value="8"
            class="button button-light"
          >
            8
          </button>
          <button
            @click="onNumberClick('9')"
            value="9"
            class="button button-light"
          >
            9
          </button>
        </div>
        <div class="grid mb-1">
          <button
            @click="onNumberClick('4')"
            value="4"
            class="button button-light"
          >
            4
          </button>
          <button
            @click="onNumberClick('5')"
            value="5"
            class="button button-light"
          >
            5
          </button>
          <button
            @click="onNumberClick('6')"
            value="6"
            class="button button-light"
          >
            6
          </button>
        </div>
        <div class="grid mb-1">
          <button
            @click="onNumberClick('1')"
            value="1"
            class="button button-light"
          >
            1
          </button>
          <button
            @click="onNumberClick('2')"
            value="2"
            class="button button-light"
          >
            2
          </button>
          <button
            @click="onNumberClick('3')"
            value="3"
            class="button button-light"
          >
            3
          </button>
        </div>
        <div class="grid mb-1">
          <button
            @click="onNumberClick('0')"
            value="0"
            class="button button-light"
          >
            0
          </button>
          <button
            class="button button-light"
            @click="onNumberClick('.')"
            value="."
          >
            .
          </button>
          <button
            @click="onOperatorClick('=')"
            value="="
            class="equal-button button"
          >
            =
          </button>
        </div>
      </div>
      <div class="sections">
        <div class="grid mb-1">
          <button class="button" @click="reset()">CE</button>
        </div>
        <div class="grid mb-1">
          <button class="button" @click="onOperatorClick('/')" value="/">
            &#247;
          </button>
        </div>
        <div class="grid mb-1">
          <button class="button" @click="onOperatorClick('*')" value="*">
            x
          </button>
        </div>
        <div class="grid mb-1">
          <button class="button" @click="onOperatorClick('-')" value="-">
            -
          </button>
        </div>
        <div class="grid mb-1">
          <button class="button" @click="onOperatorClick('+')" value="+">
            +
          </button>
        </div>
      </div>
    </div>
  </b-container>
</template>
<script>
export default {
  name: "calculator",
  data() {
    return {
      currentValue: "",
      result: 0,
      stack: [],
      exp: [],
      showExpression: false,
      showAns: false,
      operator: "",
    };
  },
  methods: {
    onNumberClick(number) {
      this.showAns = true;
      this.showExpression = false;
      if (this.operator === "=") {
        this.stack = [];
        this.currentValue = "";
      }
      if (this.stack.length) {
        const top = this.stack.length - 1;
        const topValue = this.stack[top];
        if (typeof +topValue == "number") {
          this.stack[top] = this.stack[top] + number; // top  +35
          this.currentValue += number;
        }
      } else {
        this.stack.push(number);
        this.currentValue += number;
      }
    },
    onOperatorClick(operator) {
      this.operator = operator;
      const top = this.stack.length - 1;
      if (
        this.stack[top] == "-" ||
        this.stack[top] == "+" ||
        this.stack[top] == "/" ||
        this.stack[top] == "*"
      ) {
        this.result = 0;
        this.showAns = true;
        return;
      }
      if (operator === "=") {
        const exp = this.stack.reduce((acc, value) => acc + value, "");
        this.result = eval(exp);
        this.result = this.result.toString();
        this.exp = Array.from(this.stack); // history
        this.stack = [];
        this.stack.push(this.result.toString());
        this.currentValue = this.result;
        this.showExpression = true;
        this.showAns = false;
        return;
      }
      this.showAns = true;
      this.showExpression = false;
      this.stack.push(operator);
      this.currentValue += operator;
    },
    reset() {
      this.currentValue = "";
      this.result = 0;
      this.showAns = false;
      this.showExpression = false;
      this.stack = [];
      this.exp = [];
    },
  },
};
</script>
<style scoped>
.container {
  width: 800px;
}
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 5px;
}
.board {
  height: 70px;
  border: 1px solid #e5e7e9;
  border-radius: 10px;
}
.board:active {
  border: 1px solid rgb(121, 121, 201);
}
.buttons-container {
  display: grid;
  grid-template-columns: 2fr 2fr 2fr;
  column-gap: 5px;
}
.custom-button-container {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 5px;
}
.button {
  background-color: #dadce0;
  color: black;
  border: none;
  border-radius: 5px;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  text-align: center;
  text-decoration: none;
  vertical-align: middle;
  display: inline-block;
  font-weight: 400;
  line-height: 1.5;
}
.button:active {
  box-shadow: 0 3px 10px #777;
}
.equal-button {
  background-color: #4585f4;
  color: white;
}
.equal-button:active {
  box-shadow: 0 3px 10px #4585f4;
}
.button-light {
  background-color: #f1f3f4;
}
.sections {
  width: 330px;
}
img {
  width: 25px;
}
</style>
