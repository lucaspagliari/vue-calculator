<template>
  <div class="container">
    <div class="calculator dark">
      <div class="display">123</div>

      <c-button v-for="item in items" :key="item" :value="item" :class="item">
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
      items: [
        "AC",
        "√",
        "%",
        "/",
        "7",
        "8",
        "9",
        "x",
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
    };
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
}
.dark {
  background: #262626;
  border-radius: 20px;
  color: #f1ffff;
}
.display {
  grid-column: 1 / 5;
}
</style>