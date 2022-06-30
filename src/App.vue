<script setup>
import { ref, onMounted, computed, watch } from "vue";

const meds = ref([]);
const patient_name = ref("");

const input_medicine = ref("");
const input_timing = ref(null);

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
  if (input_medicine.value.trim() === "" || input_timing.value === null) {
    return;
  }

  meds.value.push({
    content: input_medicine.value,
    timing: input_timing.value,
    done: false,
    createdAt: new Date().getTime()
  });

  input_medicine.value = "";
  input_timing.value = null;
};

const removeMed = med => {
  meds.value = meds.value.filter(t => t !== med);
};

onMounted(() => {
  patient_name.value = JSON.parse(localStorage.getItem("patient_name")) || "";
  meds.value = JSON.parse(localStorage.getItem("meds")) || [];
});
</script>

<template>
  <div class="px-4 py-6">
    <section class="flex mb-4 header">
      <div class="flex flex-col left-section">
        <span class="text-xl text-slate-800">Medication Schedule for</span>
        <input
          type="text"
          placeholder="Name here"
          v-model="patient_name"
          class="text-3xl font-medium capitalize bg-transparent focus:outline-1 focus:outline-yellow-400"
        />
      </div>
      <div class="right-section"></div>
    </section>
    <section class="p-4 rounded-lg add-med bg-sky-100">
      <form @submit.prevent="addMed">
        <h4>What's the medicine you need to drink?</h4>
        <input
          type="text"
          placeholder="e.g Paracetamol 500mg"
          v-model="input_medicine"
          class="w-full px-4 py-2 mb-4 rounded-lg"
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
            <span class="checkbox-icon"></span>
            <span class="labels">Morning, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="MorAft"
              value="Morning-After"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Morning, After Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="NooBef"
              value="Noon-Before"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Noon, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="NooAft"
              value="Noon-After"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Noon, After Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="EveBef"
              value="Evening-Before"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Evening, Before Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="EveAft"
              value="Evening-After"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Evening, After Eat</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="Night"
              value="Night"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Night, Before You Sleep</span>
          </label>

          <label>
            <input
              type="radio"
              name="drinkTime"
              id="O-w-n"
              value="Only-When-Needed"
              v-model="input_timing"
            />
            <span class="checkbox-icon"></span>
            <span class="labels">Only When Needed</span>
          </label>
        </div>

        <input
          type="submit"
          value="add medicine"
          class="w-full p-4 my-4 font-medium text-center text-white capitalize bg-indigo-600 rounded-lg hover:opacity-75"
        />
      </form>
    </section>
    <div class="py-4 meds-items">
      <div
        v-for="medic in meds_sortasc"
        :class="`meds-item ${medic.done && 'done'}`"
        :key="{ medic }"
      >
        <label>
          <input type="checkbox" v-model="medic.done" />
          <span :class="`checkbox-icon ${medic.done ? 'cleared' : ''}`"></span>
        </label>

        <div class="meds-title">
          <!-- <input type="text" v-model="medic.content" /> -->
          {{ medic.content + " --- "+ medic.timing }}
        </div>
        <div class="actions">
          <button class="delete" @click="removeMed(medic)">Delete</button>
        </div>
      </div>
    </div>

    <section class="meds-list">
      <h3>Medication Schedule</h3>
      <div class="daily-calendar">
        <div class="grid grid-cols-2 gap-2">
          <div class="calendar-block">
            <div class="calendar-title">Morning, Before Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Morning, After Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Noon, Before Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Noon, After Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Evening, Before Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Evening, After Eat</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Night</div>
          </div>
          <div class="calendar-block">
            <div class="calendar-title">Only When Needed</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="postcss" scoped>
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

input:checked ~ .checkbox-icon::after {
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

.calendar-block {
  @apply w-full h-40 rounded-lg bg-amber-100 px-4 py-2;
}

.meds-item {
  @apply flex bg-gray-100 py-2 px-4 mb-2 rounded-lg items-center space-x-2;
}

.meds-item.done .meds-title {
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

.clear-button {
  @apply px-4 py-2 bg-red-500 text-white rounded;
}
</style>
