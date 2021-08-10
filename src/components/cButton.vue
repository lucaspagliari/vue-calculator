<template>
  <div class="cButton">
    <span>
      <slot> </slot>
    </span>
  </div>
</template>

<script>
export default {
  props: {
    value: String,
  },
};
</script>

<style scoped>
/* https://getcssscan.com/css-box-shadow-examples */
.cButton {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 20px;
  box-shadow: #00000026 2.4px 2.4px 3.2px;
  border: 1px solid #2e2e2e;
  transition: all 0.1s ease-in-out;
}
.cButton:hover {
  border: 1px solid #a981d675;
}
.cButton:active {
  box-shadow: #00000026 3px 3px 6px 0px inset, #00000026 -3px -3px 6px 1px inset;
}
.cButton > span {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>