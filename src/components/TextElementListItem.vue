<template>
  <span :style="elementStyle" @click="onElementClick">{{textEl.text}}</span>
</template>

<script>
export default {
  props: {
    textEl: {
      required: true
    }
  },
  computed: {
    elementStyle() {
      const { color, fontSize, backgroundColor } = this.textEl;
      return `color: ${color}; font-size: ${fontSize}; background-color: ${backgroundColor}`;
    }
  },
  methods: {
    onElementClick() {
      this.$emit("click", this.textEl);
    }
  }
};
</script>

<style>
span {
  display: inline-block;
  border-radius: 4px;
  margin: 4px;
}
</style>
