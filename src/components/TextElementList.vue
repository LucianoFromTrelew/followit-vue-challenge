<template>
  <section>
    <article v-for="item of items" :key="item.id">
      <TextElementListItem :text-el="item" @click="onItemClick" />
      <br />
    </article>
  </section>
</template>

<script>
import TextElementListItem from "@/components/TextElementListItem";
export default {
  components: {
    TextElementListItem
  },
  props: {
    items: {
      type: Array,
      required: true
    }
  },
  methods: {
    onItemClick(textElement) {
      this.$emit("item-click", textElement);
    }
  }
};
</script>

<style></style>
