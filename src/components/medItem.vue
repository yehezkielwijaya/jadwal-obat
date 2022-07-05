<script>
import { ref } from "vue";

export default {
  name: "MedItem",
  props: {
    done: Boolean,
    content: String,
    timing: Array,
  },
  inject: ["deleteItem"],
  data() {
    return {
      isDeleted: false,
    }
  },
  methods: {
    deleteMed(content, timing) {
      this.isDeleted = true;
      this.deleteItem(content, timing);
    }
  }
};
</script>

<template>
  <div class="meds-item" :class="{done: done, deleted: isDeleted}">
    <label>
      <input type="checkbox" v-model="done" />
      <span :class="`checkbox-icon`"></span>
    </label>

    <div class="meds-title">
      {{ content }}
    </div>
    <div class="flex space-x-4 actions">
      <button class="delete" @click="deleteMed(content, timing)">X</button>
    </div>
  </div>
</template>

<style lang="postcss" scoped>
.meds-item {
  @apply flex bg-stone-50 py-2 px-3 mb-2 rounded-lg items-center space-x-2 shadow;
}

.meds-item.deleted {
  display: none;
}

.meds-item.done .meds-title{
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

input[type="radio"],
input[type="checkbox"] {
  display: none;
}

.checkbox-icon {
  width: 21px;
  height: 21px;
  border: 3px solid;
  @apply flex items-center justify-center rounded-full border-indigo-500 shadow hover:opacity-75;
}

.checkbox-icon::after {
  content: "";
  transition: 0.2s ease-in-out;
  @apply bg-indigo-500 rounded-full w-0 h-0 opacity-0 block;
}

input:checked ~ .checkbox-icon::after {
  width: 10px;
  height: 10px;
  opacity: 1;
}
</style>
