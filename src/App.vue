<script setup>
import {ref, computed} from "vue";

const header = ref("Shopping List App");
const editing = ref(false);
const items = ref([
  {id: 1, label: "10 party hats", purchased: true, highPriority: true},
  {id: 2, label: "2 board games", purchased: true, highPriority: false},
  {id: 3, label: "20 cups", purchased: false, highPriority: true},
]);
const reversedItems = computed(()=>  [...items.value].reverse());
const newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1 ,
    label: newItem.value,
    highPriority: newItemHighPriority.value
  });

  newItem.value = "";
  newItemHighPriority.value = false;
}
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
  newItemHighPriority.value = false;
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
}
</script>

<template>
  <div class="header" >
    <h1>{{ header }}</h1>
    <button v-if="editing" @click="doEdit(false)" class="btn" >Cancel</button>
    <button v-else @click="doEdit(true)" class="btn btn-primary" >Add Item</button>
  </div>
  <form
      class="add-item-form"
      v-if="editing"
      @submit.prevent="saveItem" >
    <input
        v-model.trim="newItem"
        type="text"
        placeholder="Add an item"
    >
    Priority:
    <label>
      <input v-model="newItemHighPriority" type="checkbox" >High Priority
    </label>
    <button
        class="btn btn-primary"
        :disabled="newItem.length < 5"
      >Save Item</button>
  </form>
  <ul>
    <li
        v-for="(item, index) in reversedItems"
        :key="item.id"
        :class="{strikeout: item.purchased, priority: item.highPriority}"
        @click="togglePurchased(item)"
    >{{ item.label }}</li>
  </ul>
  <p v-if="!items.length" >
    Nothing to see here
  </p>
</template>

<style>
@import 'assets/main.css';
</style>
