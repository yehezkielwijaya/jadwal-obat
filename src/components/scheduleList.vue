<template>
  <section class="meds-list">
    <h3 class="mb-2 text-xl font-bold text-center">Medication Schedule</h3>
    <div class="daily-calendar">
      <div class="grid gap-4 md:grid-cols-2">
        <TimingBox :dataList="dataList" title="Morning, Before Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Morning, After Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Noon, Before Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Noon, After Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Evening, Before Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Evening, After Eat">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Night, Before You Sleep">
        </TimingBox>
        <TimingBox :dataList="dataList" title="Only When Needed">
        </TimingBox>
      </div>
    </div>
  </section>
</template>

<script>
import { ref, onMounted, computed, watch } from "vue";

import MedItem from './medItem.vue';
import TimingBox from "./timingBlock.vue";

const meds_filtered = ref([]);

const filterList = (cat) => {
  dataList.value = dataList.value.filter(t => t !== cat);
};

export default {
  name: 'ScheduleList',
  props: {
    dataList: Array,
  },
  components: {
    TimingBox,
  },
}
</script>
