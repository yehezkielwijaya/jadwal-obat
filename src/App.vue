<script setup>
import { ref, onMounted, computed, watch } from "vue";

const meds = ref([
  {id: 1, content: 'Paracetamol', timing: ['1B','2A'], done: false, createdAt: ''},
  {id: 2, content: 'Ibuprofen', timing: ['4','3A'], done: false, createdAt: ''},
  {id: 3, content: 'Paracetamol', timing: ['5','2B'], done: false, createdAt: ''},
  {id: 4, content: 'God', timing: ['1B','3B'], done: false, createdAt: ''},
  {id: 5, content: 'Prayer', timing: ['1B','2B'], done: false, createdAt: ''},
]);
const patient_name = ref("");

const input_medicine = ref("");
const input_timing = ref([]);

const add_modal_isOpen = ref(false);

const meds_sortasc = computed(() =>
  meds.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

watch(patient_name, newVal => {
  localStorage.setItem("patient_name", JSON.stringify(newVal));
});

watch(
  meds,
  newVal => {
    localStorage.setItem("meds", newVal);
    console.log(meds.value);
  },
  { deep: true }
);

const addMed = () => {
  if (input_medicine.value.trim() === "" || input_timing.value === []) {
    return;
  }

  meds.value.push({
    content: input_medicine.value,
    timing: input_timing.value,
    done: false,
    createdAt: new Date().getTime()
  });

  input_medicine.value = "";
  input_timing.value = [];
};

const removeMed = (index) => {
  return meds.value.splice(index, 1);
  // meds.value = meds.value.filter(t => t !== med);
};

onMounted(() => {
  patient_name.value = JSON.parse(localStorage.getItem("patient_name")) || "";
  meds.value =  JSON.parse(localStorage.getItem("meds")) || {};
});
</script>

<script>
import ScheduleList from "./components/scheduleList.vue";
import MedItem from "./components/medItem.vue"

export default {
  name: 'App',
  components: { 
    ScheduleList,
    MedItem
  }
}
</script>

<template>
  <div class="relative px-4 py-6">
    <section class="flex mb-4 space-x-4 header">
      <div class="flex flex-col w-full left-section">
        <span class="text-xl text-slate-800">Medication Schedule for</span>
        <input type="text" placeholder="Name here" v-model="patient_name"
          class="text-3xl font-medium capitalize bg-transparent focus:outline-1 focus:outline-yellow-400" />
      </div>
      <div class="flex h-fit right-section">
        <button @click="add_modal_isOpen = true" class="add-medicine">
          + Add
        </button>
      </div>
    </section>
    <section
      :class="`fixed top-0 left-0 w-full overflow-hidden flex items-center bg-slate-900 bg-opacity-90 ${add_modal_isOpen ? 'h-screen p-8' : 'h-0'}`">
      <button @click="add_modal_isOpen = false" class="absolute add-medicine top-4 right-4">X</button>
      <form @submit.prevent="addMed" class="p-4 rounded-lg add-med bg-sky-100">
        <h4>What's the medicine you need to drink?</h4>
        <input type="text" placeholder="e.g Paracetamol 500mg" v-model="input_medicine"
          class="w-full px-4 py-2 mb-4 rounded-lg" />
        <h4>When do you need to drink the medicine?</h4>
        <div class="options">
          <label>
            <input type="checkbox" id="MorBef" value="1B" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Morning, Before Eat</span>
          </label>

          <label>
            <input type="checkbox" id="MorAft" value="1A" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Morning, After Eat</span>
          </label>

          <label>
            <input type="checkbox" id="NooBef" value="2B" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Noon, Before Eat</span>
          </label>

          <label>
            <input type="checkbox" id="NooAft" value="2A" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Noon, After Eat</span>
          </label>

          <label>
            <input type="checkbox" id="EveBef" value="3B" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Evening, Before Eat</span>
          </label>

          <label>
            <input type="checkbox" id="EveAft" value="3A" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Evening, After Eat</span>
          </label>

          <label>
            <input type="checkbox" id="Night" value="4" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Night, Before You Sleep</span>
          </label>

          <label>
            <input type="checkbox" id="O-w-n" value="5" v-model="input_timing" />
            <span class="checkbox-icon"></span>
            <span class="labels">Only When Needed</span>
          </label>
        </div>

        <input type="submit" @click="add_modal_isOpen = false" value="add medicine"
          class="w-full p-4 my-4 font-medium text-center text-white capitalize bg-indigo-600 rounded-lg hover:opacity-75" />
      </form>
    </section>
    <!-- <MedItem
        v-for="(med, index) in meds_sortasc"
        :key="{ med }"
        :done="med.done"
        :content="med.content"
        :timing="med.timing"
        @remove="removeMed(index)"
      /> -->
    <scheduleList :medList="meds_sortasc" />
    <section class="footer">
      <div class="quote"> “A joyful heart is good medicine, but a broken spirit dries up the bones.” - Proverbs 17:22</div>
      <small>Created with love by <a href="http://yehezkielwijaya.vercel.app" target="_blank" class="font-medium text-yellow-700">Yehezkiel Wijaya</a></small>
    </section>
  </div>
</template>

<style lang="postcss" scoped>
.add-medicine {
  @apply px-4 py-2 bg-indigo-600 text-white rounded-lg font-medium hover:opacity-75 whitespace-nowrap;
}

.add-med h4 {
  font-size: 1.125em;
  margin-bottom: 4px;
}

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

.options label {
  @apply flex items-center space-x-2;
}

.options .labels {
  @apply text-sm;
}

.clear-button {
  @apply px-4 py-2 bg-red-500 text-white rounded;
}

.footer {
  @apply flex flex-col items-center;
}

.quote {
  @apply px-6 py-4 bg-amber-50 my-4 rounded italic font-medium text-lg;
}
</style>
