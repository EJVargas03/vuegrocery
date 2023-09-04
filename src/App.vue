<script setup>
  import { ref } from 'vue'
  import { useStorage } from '@vueuse/core'
  import { nanoid } from 'nanoid'
  import confetti from 'canvas-confetti'

  const newGrocery = ref('')
  const groceries = useStorage('groceries', [])

  const addGrocery = () => {
    if (newGrocery.value) {
      groceries.value.push({id: nanoid(), name: newGrocery.value})
      newGrocery.value = ''
    }
  }

  const deleteGrocery = id => {
    const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
    groceries.value.splice(removeIndex, 1)
    confetti({ particleCount: 10, spread: 8000, origin: { y: 1 } })
  }
</script>

<template>
  <main>
    <h1 class="title">Vue Grocery List</h1>
    <form class="newGroceryForm" @click.prevent="addGrocery">
      <input
      id="newGrocery"
      autocomplete="off"
      type="text"
      placeholder="* Type you item here "
      v-model="newGrocery"
      />
      <button type="submit">Add</button>
    </form>
    <h3>Pending Items: {{ groceries.length }}</h3>
      <ul>
        <li v-for="grocery in groceries" @click="deleteGrocery(grocery.id)">
          {{ grocery.name }}
        </li>
      </ul>
    <h2 class="guide">How to use this program</h2>
      <ol id="guideList">
          <li class="guideItem">1. Type in your item</li>
          <li class="guideItem">2. Cick the "add" button</li>
          <li class="guideItem">3. Click the item on the list to remove it</li>
          <li class="guideItem">4. Confetti !!!</li>
      </ol>
  </main>
</template>

<style  scoped>
  main {
      @apply mt-8 flex flex-col justify-center items-center gap-8;
    .title {
        @apply m-2 text-6xl font-light tracking-wider text-purplish;
    }
    .guide {
        @apply flex-col pt-5 text-comment ;
    }
    form{
      @apply flex focus-within:ring-8 focus-within:ring-purplish focus-within:rounded-lg;
      input {
        @apply bg-white text-comment p-2 w-80 text-xl rounded-l-md outline-none;
      }
      button {
        @apply bg-purplish text-background p-2 text-2xl font-bold rounded-r-md;
        &:hover {
          @apply bg-bluish text-purple-50;
      }
    }
  }
  ul{
    @apply flex flex-col items-center justify-center rounded-lg bg-comment;
    li {
        @apply bg-white text-background m-2 p-2 w-96 text-center;
        &:hover{
          @apply text-purplish;
        }
      }
    }
    ol{
    @apply flex-col text-comment ;
    li {
      @apply text-comment m-1 pl-16 w-96;
    }
  }
}
</style>
