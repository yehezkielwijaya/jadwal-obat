<template>
  <div :class="`meds-item ${done && 'done'}`">
    <label>
      <input type="checkbox" v-model="done" />
      <span :class="`checkbox-icon ${done ? 'cleared' : ''}`"></span>
    </label>

    <div class="meds-title">
      {{ content }}
    </div>
    <div class="actions">
      <button class="delete" @click="deleteItem">X</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MedItem',
  props: {
    done: String,
    content: String,
    timing: String,
  },
  deleteItem() {
    this.$emit('deleteItem');
  }
}
</script>

<style lang="postcss" scoped>

input[type="radio"],
input[type="checkbox"] {
  display: none;
}

.checkbox-icon {
  width: 21px;
  height: 21px;
  border: 3px solid;
  @apply flex items-center justify-center rounded-full border-indigo-600 shadow hover:opacity-75;
}

.checkbox-icon::after {
  content: "";
  transition: 0.2s ease-in-out;
  @apply bg-indigo-600 rounded-full w-0 h-0 opacity-0 block;
}

input:checked~.checkbox-icon::after {
  width: 10px;
  height: 10px;
  opacity: 1;
}
.meds-item {
  @apply flex bg-green-100 py-2 px-4 mb-2 rounded-lg items-center space-x-2;
}

.meds-item.done {
  @apply line-through opacity-75;
}

.meds-title {
  @apply w-full capitalize;
}

.meds-title input {
  @apply w-full border-0 outline-1 outline-yellow-400 text-black p-2 bg-transparent;
}

.meds-item .delete {
  @apply py-2 px-4 rounded bg-red-500 text-white hover:opacity-75;
}

</style>