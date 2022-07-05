<script setup>
import { ref, onMounted, computed, watch } from "vue";

const meds = ref([
  {content: 'Paracetamol 500mg', timing: ['1B','2A','3B'], done: false, createdAt: '1'}, 
  {content: 'Ascardia 100mg', timing: ['1B','2B'], done: false, createdAt: '2'}, 
]);
const patient_name = ref("");

const input_medicine = ref("");
const input_timing = ref([]);

const add_modal_isOpen = ref(false);

watch(patient_name, newVal => {
  localStorage.setItem("patient_name", newVal);
});

watch(meds, newVal => {
    localStorage.setItem("meds",  JSON.stringify(newVal));
  }, { deep: true }
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

const meds_sortasc = computed(() => meds.value.sort((a, b) => {
  return a.createdAt - b.createdAt;
}));

const modified_array = computed(() =>
  meds_sortasc.value.flatMap(({ timing, ...r }) =>
    timing.map((t) => ({ ...r, timing: [t] }))
  )
);

onMounted(() => {
  patient_name.value = localStorage.getItem("patient_name") || "";
  meds.value =  JSON.parse(localStorage.getItem("meds")) || [];
});
</script>

<script>
import ScheduleList from "./components/scheduleList.vue";

export default {
  name: 'App',
  components: { 
    ScheduleList,
  }
}
</script>

<template>
  <div class="relative max-w-screen-lg px-4 py-6 md:mx-auto">
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
{{modified_array}}

    <section
      :class="`fixed top-0 left-0 w-full overflow-hidden flex justify-center items-center bg-slate-900 bg-opacity-90 ${add_modal_isOpen ? 'h-screen p-8' : 'h-0'}`">
      <button @click="add_modal_isOpen = false" class="absolute add-medicine top-4 right-4">X</button>
      <form @submit.prevent="addMed" class="p-4 rounded-lg bg-sky-50 add-med">
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
          class="w-full p-4 my-4 font-medium text-center text-white capitalize bg-blue-500 rounded-lg hover:opacity-75" />
      </form>
    </section>
    <scheduleList :medList="modified_array" />
    <section class="footer">
      <div class="quote"> “A joyful heart is good medicine, but a broken spirit dries up the bones.” - Proverbs 17:22</div>
      <small>Created with love, vue, and tailwindcss by <a href="http://yehezkielwijaya.vercel.app" target="_blank" class="font-medium text-yellow-700">Yehezkiel Wijaya</a></small>
    </section>
  </div>
</template>

<style lang="postcss" scoped>
.add-medicine {
  @apply px-4 py-2 bg-blue-500 text-white rounded-lg font-medium hover:opacity-75 whitespace-nowrap;
}

.add-med h4 {
  font-size: 1.125em;
  margin-bottom: 4px;
  font-weight: 500;
}

input[type="radio"],
input[type="checkbox"] {
  display: none;
}

.checkbox-icon {
  width: 21px;
  height: 21px;
  border: 3px solid;
  @apply flex items-center justify-center rounded-full border-blue-500 shadow hover:opacity-75;
}

.checkbox-icon::after {
  content: "";
  transition: 0.2s ease-in-out;
  @apply bg-blue-500 rounded-full w-0 h-0 opacity-0 block;
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
  @apply px-6 py-4 bg-green-200 my-4 rounded italic font-medium text-lg;
}
</style>
