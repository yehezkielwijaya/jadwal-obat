<template>
  <div class="calendar-block">
    <div class="calendar-title">{{ title }}</div>
    <div class="calendar-list">
      <slot name="scheduleList" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'ScheduleBox',
  props: {
    title: String,
  },
  deleteItem() {
    this.$emit('deleteItem');
  }
}
</script>

<style lang="postcss" scoped>
.calendar-block {
  @apply w-full flex flex-col rounded-lg bg-gray-100 overflow-hidden shadow-md;
}

.calendar-title {
  @apply font-medium text-left text-sm mb-2 px-4 py-2 bg-amber-600 text-white;
}

.calendar-list {
  @apply min-h-[100px] px-4 py-2;
}

</style>