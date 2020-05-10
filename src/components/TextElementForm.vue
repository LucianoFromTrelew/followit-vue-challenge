<template>
  <form @submit.prevent="onSubmit">
    <label for="text">Text:</label>
    <template v-if="isEditting">
      <input
        id="text"
        name="text"
        type="text"
        :value="item.text"
        ref="editInput"
      />
      <section>
        <button @click.prevent="onChangeColorClick">Change color</button>
        <button @click.prevent="onChangeFontSizeClick">Change font size</button>
        <button @click.prevent="onChangeBackgroundColorClick">
          Change background color
        </button>
      </section>
      <button @click="onCancel">Cancel</button>
    </template>
    <input v-else id="text" name="text" type="text" v-model="newItem.text" />
    <button type="submit" :disabled="!canSubmit">{{ submitButtonText }}</button>
  </form>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      newItem: { text: "" }
    };
  },
  computed: {
    isEditting() {
      return this.item;
    },
    canSubmit() {
      if (!this.isEditting) {
        const canSubmit = Boolean(this.newItem.text.length);
        return canSubmit;
      }
      return true;
    },
    submitButtonText() {
      if (!this.isEditting) return "Create";
      return "Update";
    }
  },
  methods: {
    onSubmit() {
      if (!this.isEditting) {
        const { text } = this.newItem;
        this.$emit("create", { text });
        this.newItem.text = "";
      } else {
        // eslint-disable-next-line no-unused-vars
        const { text, ...rest } = this.item;
        this.$emit("update", { text: this.$refs.editInput.value, ...rest });
      }
    },
    onChangeColorClick() {
      const { color, ...rest } = this.item;
      if (color === "red") {
        this.$emit("update", { color: "blue", ...rest });
      } else {
        this.$emit("update", { color: "red", ...rest });
      }
    },
    onChangeFontSizeClick() {
      const { fontSize, ...rest } = this.item;
      if (fontSize === "40px") {
        this.$emit("update", { fontSize: "60px", ...rest });
      } else {
        this.$emit("update", { fontSize: "40px", ...rest });
      }
    },
    onChangeBackgroundColorClick() {
      const { backgroundColor, ...rest } = this.item;
      if (backgroundColor === "lightgray") {
        this.$emit("update", { backgroundColor: "pink", ...rest });
      } else {
        this.$emit("update", { backgroundColor: "lightgray", ...rest });
      }
    },
    onCancel() {
      this.$emit("cancel");
    }
  }
};
</script>

<style>
label,
form,
section,
button {
  margin: 8px;
}
</style>
