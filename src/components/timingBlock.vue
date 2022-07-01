<template>
  <div class="timing-block">
    <div class="timing-title">{{ title }}</div>
    <div class="timing-list">
      {{dataList}}
      <MedItem
        v-for="medic in dataList"
        :key="{ medic }"
        :done="medic.done"
        :content="medic.content"
        :timing="medic.timing"
        @deleteItem="removeMed(medic)"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "TimingBox",
  props: {
    title: String,
    dataList: Array,
  }
};
</script>

<style lang="postcss" scoped>
.timing-block {
  @apply w-full flex flex-col rounded-lg bg-gray-100 overflow-hidden shadow-md;
}

.timing-title {
  @apply font-medium text-left text-sm mb-2 px-4 py-2 bg-yellow-400;
}

.timing-list {
  @apply min-h-[150px] p-2;
}
</style>
