<script setup lang="ts">
import { ref, computed } from 'vue'

const header = ref<string>('Shopping List App')

const items = ref<
  {
    id: number
    label: string
    purchased: boolean
    highPriority: boolean
  }[]
>([
  {
    id: 1,
    label: 'Apples',
    purchased: true,
    highPriority: false,
  },
  {
    id: 2,
    label: 'Bananas',
    purchased: false,
    highPriority: true,
  },
  {
    id: 3,
    label: 'Oranges',
    purchased: true,
    highPriority: false,
  },
])
const newItem = ref<string>('')

const newItemHighPriority = ref<boolean>(false)

const editing = ref<boolean>(false)

const reversedItems = computed<
  {
    id: number
    label: string
    purchased: boolean
    highPriority: boolean
  }[]
>(() => [...items.value].reverse())

const saveItem = (): void => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    purchased: false,
    highPriority: newItemHighPriority.value,
  })
  newItem.value = ''
  newItemHighPriority.value = false
}
const doEdit = (e: boolean): void => {
  editing.value = e
  newItem.value = ''
}
const togglePurchase = (item: {
  id: number
  label: string
  purchased: boolean
  highPriority: boolean
}): void => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button>
  </div>
  <form v-if="editing" class="add-item-form" @submit.prevent="saveItem()">
    <input v-model.trim="newItem" placeholder="Add new item" />
    <label>
      <input type="checkbox" value="vanilla" v-model="newItemHighPriority" />
      High Priority
    </label>
    <button :disabled="newItem.length < 5" class="btn btn-primary">Save Item</button>
  </form>
  <ul>
    <li
      v-for="(item, index) in reversedItems"
      @click="togglePurchase(items[index])"
      :key="item.id"
      class="static-class"
      :class="{
        strikeout: item.purchased,
        priority: item.highPriority,
      }"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing to see here</p>
</template>
