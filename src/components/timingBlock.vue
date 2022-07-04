<template>
  <div class="timing-block">
    <div class="timing-title">{{ title }}</div>
    <div class="timing-list">
      <MedItem
        v-for="(med, index) in medicList"
        :key="{ med }"
        :done="med.done"
        :content="med.content"
        :timing="med.timing"
        @remove="removeMed(index)"
      />
    </div>
  </div>
</template>

<script>
import MedItem from "./medItem.vue"
export default {
  name: "TimingBox",
  props: {
    title: String,
    medicList: Array,
  },
  components: {
    MedItem,
  }
};
</script>

<style lang="postcss" scoped>
.timing-block {
  @apply w-full flex flex-col rounded-lg bg-gray-50 overflow-hidden shadow-md;
}

.timing-title {
  @apply font-medium text-left text-sm mb-2 px-4 py-2 bg-indigo-600 text-white;
}

.timing-list {
  @apply min-h-[150px] p-2
}
</style>
