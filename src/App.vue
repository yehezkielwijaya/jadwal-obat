<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const meds = ref([]);
const patient_name = ref('');

const input_medicine = ref('');
const input_timing = ref(null);


const meds_sortasc = computed(() => meds.value.sort((a, b) => {
  return a.createdAt - b.createdAt;
}));

const addMed = () => {
  if (input_medicine.value.trim() === '' || input_timing.value === null) {
    return
  }

  meds.value.push({
    name: input_medicine.value,
    timing: input_timing.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_medicine.value = ''
  input_timing.value =null
}

const removeMed = medicine => {
  meds.value = meds.value.filter(t => t !== medicine)
}

watch(meds, (newVal) => {
  localStorage.setItem('meds', newVal);
}), { deep: true}

watch(patient_name, (newVal) => {
  localStorage.setItem('patient_name', JSON.stringify(newVal));
}), { deep: true}

onMounted(() => {
  patient_name.value = localStorage.getItem('patient_name') || '';
  meds.value = JSON.parse(localStorage.getItem('meds')) || [];
})
</script>

<template>
  <div class="px-4 py-6">
    <section class="flex flex-col mb-4">
      <span class="text-xl text-slate-800">Medication Schedule for</span>
      <input
        type="text"
        placeholder="Name here"
        v-model="patient_name"
        class="text-3xl font-medium capitalize bg-transparent focus:outline-1 focus:outline-yellow-400"
      />
    </section>
    <section class="p-4 rounded-lg add-med bg-sky-100">
      <form @submit.prevent="addMed">
        <h4>What's the medicine you need to drink?</h4>
        <input
          type="text"
          placeholder="e.g Paracetamol 500mg"
          v-model="input_medicine"
          class="w-full px-4 py-2 rounded-lg"
        />
        <h4>When do you need to drink the medicine?</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="drinkTime"
              id="MorBef"
              value="Morning-Before"
              v-model="input_timing"
            />
            <span>Morning, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="MorAft"
              value="Morning-After"
              v-model="input_timing"
            />
            <span>Morning, After Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="NooBef"
              value="Noon-Before"
              v-model="input_timing"
            />
            <span>Noon, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="NooAft"
              value="Noon-After"
              v-model="input_timing"
            />
            <span>Noon, After Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="EveBef"
              value="Evening-Before"
              v-model="input_timing"
            />
            <span>Evening, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="EveAft"
              value="Evening-After"
              v-model="input_timing"
            />
            <span>Evening, After Eat</span>
          </label>

          <label>
            <input type="radio" name="drinkTime" id="Night" value="Night" v-model="input_timing" />
            <span>Night, Before You Sleep</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="O-w-n"
              value="Only-When-Needed"
              v-model="input_timing"
            />
            <span>Only When Needed</span>
          </label>
        </div>

        <input
          type="submit"
          value="add medicine"
          class="w-full p-4 my-4 font-medium text-center text-white capitalize bg-indigo-600 rounded-lg"
        />
      </form>
    </section>
    <div class="py-4 meds-items">
      <div v-for="meds in meds_sortasc" :class="`meds-item ${meds.done && 'done'}`" :key="{meds}">

        <label>
          <input type="checkbox" v-model="meds.done" />
          <span :class="`checkbox-icon ${meds.done ? 'cleared' : '' }`"></span>
        </label>

        <div class="meds-title">
          <input type="text" v-model="meds.name" />
          {{meds.name}}
        </div>
        <div class="meds-title">
          {{meds.timing}}
        </div>
        <div class="actions">
          <button class="delete" @click="removeMed(medicine)">Delete</button>
        </div>
      </div>
    </div>

    <section class="hidden meds-list">
      <h3>Medication Schedule</h3>
      <div class="daily-calendar">
        <div class="grid grid-cols-2 gap-2">
          <div class="calendar-block">Morning, Before Eat</div>
          <div class="calendar-block">Morning, After Eat</div>
          <div class="calendar-block">Noon, Before Eat</div>
          <div class="calendar-block">Noon, After Eat</div>
          <div class="calendar-block">Evening, Before Eat</div>
          <div class="calendar-block">Evening, After Eat</div>
          <div class="calendar-block">Night</div>
          <div class="calendar-block">Only When Needed</div>
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="postcss" scoped>
.options label{
  @apply space-x-2;
}
.calendar-block {
  @apply w-full h-40 rounded-lg bg-amber-100 px-4 py-2;
}

.meds-item {
  @apply flex bg-gray-100 py-2 px-4 mb-2 rounded-lg items-center space-x-2;
}

.meds-title {
  @apply w-full;
}

.meds-title input {
  @apply w-full border border-2 border-red-500 text-red-500 bg-transparent ;
}

.meds-item .delete {
  @apply py-2 px-4 rounded bg-red-500 text-white;
}


</style>
