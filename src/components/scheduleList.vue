<script>
import TimingBox from "./timingBlock.vue";

export default {
  name: "ScheduleList",
  props: {
    medList: Array,
  },
  components: {
    TimingBox
  },
  methods: {
    timingFilter(base, filter) {
      return base.filter((item) => {
        return (item.timing.indexOf(filter) >= 0);
      })
    }
  },
};

</script>

<template>
  <section class="meds-list">
    <h3 class="mb-2 text-xl font-bold text-center">Medication Schedule</h3>
    <div class="daily-calendar">
      <div class="grid gap-4 md:grid-cols-2">
        <TimingBox name="MorB" :medicList="timingFilter(medList, '1B')" title="Morning, Before Eat"> </TimingBox>
        <TimingBox name="MorA" :medicList="timingFilter(medList, '1A')" title="Morning, After Eat"> </TimingBox>
        <TimingBox name="NooB" :medicList="timingFilter(medList, '2B')" title="Noon, Before Eat"> </TimingBox>
        <TimingBox name="NooA" :medicList="timingFilter(medList, '2A')" title="Noon, After Eat"> </TimingBox>
        <TimingBox name="EveB" :medicList="timingFilter(medList, '3B')" title="Evening, Before Eat">
        </TimingBox>
        <TimingBox name="EveA" :medicList="timingFilter(medList, '3A')" title="Evening, After Eat"> </TimingBox>
        <TimingBox name="Nig" :medicList="timingFilter(medList, '4')" title="Night, Before You Sleep">
        </TimingBox>
        <TimingBox name="Own" :medicList="timingFilter(medList, '5')" title="Only When Needed"> </TimingBox>
      </div>
    </div>
  </section>
</template>
