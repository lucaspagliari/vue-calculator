<template>
  <div class="container">
    <div class="calculator dark">
      <div class="display">
        <template v-for="(item, i) in historic" :key="i">
          <span class="dimmed-color">
            {{ item }}
          </span>
        </template>
        <span>{{ operation }}</span>
      </div>

      <c-button
        v-for="item in items"
        :key="item"
        :value="item"
        class="btn"
        @pressed="buttonClicked"
      >
        {{ item }}
      </c-button>
    </div>
  </div>
</template>

<script>
import cButton from "./cButton.vue";

export default {
  name: "Calculator",
  components: {
    cButton,
  },
  data() {
    return {
      operation: "",
      items: [
        "AC",
        "%",
        "/",
        "⇽",
        "7",
        "8",
        "9",
        "⨉",
        "4",
        "5",
        "6",
        "-",
        "1",
        "2",
        "3",
        "+",
        "0",
        ".",
        "=",
      ],
      historic: [],
      operationFunction: {
        "/": (a, b) => a / b,
        "⨉": (a, b) => a * b,
        "-": (a, b) => a - b,
        "+": (a, b) => a + b,
      },
      operationOrder: ["/", "⨉", "-", "+"],
    };
  },
  computed: {
    operationArray: {
      get() {
        if (this.operation) {
          return this.operation.split(" ");
        }
        return [];
      },
      set(value) {
        this.operation = value.join(" ");
      },
    },
  },
  methods: {
    buttonClicked(key) {
      switch (key) {
        case "=":
          this.equalsPressed();
          break;
        case "AC":
          this.clearPressed();
          break;
        case "%":
          this.percentPressed();
          break;
        case ".":
          this.operation += key;
          break;
        case "⇽":
          this.dealsWithDelete();
          break;
        default:
          if (!isNaN(parseInt(key))) {
            this.operation += key;
          } else {
            this.dealsWithSign(key);
          }
          break;
      }
    },
    clearPressed() {
      this.operation = "";
      this.historic = [];
    },
    dealsWithSign(key) {
      if (this.operation.endsWith(" ")) {
        this.operation = this.operation.substr(0, this.operation.length - 3);
      }
      if (!this.operation) {
        this.operation += "1";
      }
      this.operation += ` ${key} `;
    },
    dealsWithDelete() {
      this.operation = this.operation.trim();
      this.operation = this.operation
        .substr(0, this.operation.length - 1)
        .trim();
    },
    percentPressed() {
      const operation = [...this.operationArray];
      const length = operation.length;
      const value = operation[length - 1];
      if (!isNaN(parseInt(value))) {
        operation[length - 1] = parseFloat(value) / 100;
        this.operationArray = operation;
      }
    },
    equalsPressed() {
      let operations = [...this.operationArray];
      if (operations.length > 2) {
        let usedOperations = new Set();
        this.operationOrder.forEach((item) => {
          const index = operations.indexOf(item);
          if (index >= 0) {
            usedOperations.add(item);
          }
        });

        usedOperations.forEach((item) => {
          while (operations.includes(item)) {
            const index = operations.indexOf(item);

            const f = this.operationFunction[item];
            const a = parseFloat(operations[index - 1]);
            const b = parseFloat(operations[index + 1]);

            operations.splice(index + 1, 1);
            operations[index] = f(a, b);
            operations.splice(index - 1, 1);
          }
        });
        this.addHistoric(this.operation);
        this.operationArray = operations;
      }
    },
    addHistoric(item) {
      if (this.historic.length > 3) {
        this.historic.shift();
      }
      this.historic.push(item);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  margin: auto;
  max-width: 330px;
  height: 100%;
}
.calculator {
  display: grid;
  grid-template-rows: 40% repeat(5, 1fr);
  grid-template-columns: repeat(4, 1fr);
  justify-content: center;
  align-items: center;
  justify-items: center;
  gap: 10px;
  width: 100%;
  height: 500px;
  padding: 10px;
  align-items: center;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px,
    rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}
.dark {
  background: #262626;
  border-radius: 20px;
  color: #f1ffff;
}
.display {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: flex-end;
  width: 100%;
  height: 100%;
  padding: 15px;
  grid-column: 1 / 5;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  font-size: 30px;
}
.btn:nth-last-of-type(3) {
  grid-column: 1 / 3;
}
.dimmed-color {
  opacity: 0.5;
  font-size: 25px;
}
</style>