<script>
import MedItem from "./medItem.vue"
export default {
  name: "TimingBox",
  props: {
    title: String,
    medicList: Array,
    name: String,
  },
  components: {
    MedItem,
  },
  methods: {
    getClass() {
      return {
        'morb': this.name === "MorB",
        'mora': this.name === "MorA",
        'noob': this.name === "NooB",
        'nooa': this.name === "NooA",
        'eveb': this.name === "EveB",
        'evea': this.name === "EveA",
        'night': this.name === "Nig",
        'own': this.name === "Own",
      }
    }
  },
};
</script>

<template>
  <div class="timing-block">
    <div class="flex justify-between">
      <div class="timing-title">{{ title }}</div>
      <!-- <button class="mark-all">✓✓</button> -->
    </div>
    <div class="timing-list" :class="getClass()">
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

<style lang="postcss" scoped>
.timing-block {
  @apply w-full flex flex-col rounded-lg bg-gray-50 overflow-hidden shadow-md;
}

.timing-title {
  @apply w-full font-medium text-left text-sm px-4 py-2 bg-blue-600 text-white;
}

.mark-all {
  @apply w-fit bg-green-600 text-white px-4 py-2 pr-6;
  white-space: nowrap;
  letter-spacing: -8px;
}

.timing-list {
  @apply min-h-[160px] p-2 pt-4 h-full;
}

.morb {
  @apply bg-emerald-200;
}
.mora {
  @apply bg-emerald-200;
}
.noob {
  @apply bg-amber-200;
}
.nooa {
  @apply bg-amber-200;
}
.eveb {
  @apply bg-indigo-200;
}
.evea {
  @apply bg-indigo-200;
}
.night {
  @apply bg-slate-300;
}
.own {
  @apply bg-rose-300;
}
</style>
