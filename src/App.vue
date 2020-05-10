<template>
  <div id="app">
    <TextElementForm
      :item="itemToEdit"
      @create="onTextElementCreated"
      @update="onTextElementUpdated"
      @cancel="onFormCancel"
    />
    <TextElementList :items="items" @item-click="onItemClick" />
    <button @click="onExportClick">Export</button>
  </div>
</template>

<script>
const createDefaultTextElement = text => ({
  text,
  id: uuid(),
  color: "red",
  fontSize: "40px",
  backgroundColor: "lightgray"
});
import { v4 as uuid } from "uuid";
import TextElementForm from "@/components/TextElementForm";
import TextElementList from "@/components/TextElementList";
export default {
  components: {
    TextElementList,
    TextElementForm
  },
  name: "App",
  data() {
    return {
      itemToEdit: null,
      items: ["Hi", "My lovely", "little", "Ponny"].map(
        createDefaultTextElement
      )
    };
  },
  methods: {
    onItemClick(item) {
      this.itemToEdit = item;
    },
    onTextElementCreated(item) {
      const { items } = this;
      this.items = [...items, createDefaultTextElement(item.text)];
    },
    onTextElementUpdated(item) {
      this.items = this.items.map(i => {
        if (i.id === item.id) return item;
        return i;
      });
      this.reset();
    },
    onFormCancel() {
      this.reset();
    },
    reset() {
      this.itemToEdit = null;
    },
    onExportClick() {
      function haveSameStyle(item1, item2) {
        return (
          item1.color === item2.color &&
          item1.fontSize === item2.fontSize &&
          item1.backgroundColor === item2.backgroundColor
        );
      }
      const { items } = this;
      const objs = [];
      // Grouping by adjacent same style
      for (let index = 0; index < items.length; index++) {
        const item = items[index];
        if (index === 0) {
          // eslint-disable-next-line no-unused-vars
          const { id, ...rest } = item;
          objs.push({ ...rest });
        } else {
          const prevItem = objs[objs.length - 1];
          if (haveSameStyle(item, prevItem)) {
            objs[objs.length - 1].text = `${prevItem.text} ${item.text}`;
          } else {
            // eslint-disable-next-line no-unused-vars
            const { id, ...rest } = item;
            objs.push({ ...rest });
          }
        }
      }
      console.log(objs);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
