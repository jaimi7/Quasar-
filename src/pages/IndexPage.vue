<script setup>
import { ref, watch } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();

const data = ref({
  counter: 0,
  name: "",
});

const savedData = $q.localStorage.getItem("data");
if (savedData) data.value = savedData;

watch(data, (value) => {
  $q.localStorage.set("data", value);
});

const increaseCounter = () => {
  data.value.counter++;
};
const decreaseCounter = () => {
  if (data.value.counter > 0) data.value.counter--;
};
const resetCounter = () => {
  data.value.counter = 0;
};

const handlePan = (e) => {
  if (e.delta.y < 0) increaseCounter();
  else decreaseCounter();
};
</script>

<template>
  <q-page v-touch-pan.vertical.prevent.mouse="handlePan" class="flex flex-center text-white">
    <div class="row">
      <q-input
        v-model="data.name"
        input-class="text-center text-h5 text-white"
        color="blue"
        placeholder="Counter"
        filled
      />
    </div>
    <div class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          v-touch-repeat:300:300:300:300:100.mouse="decreaseCounter"
          icon="remove"
          size="xl"
          round
          @click="decreaseCounter"
        />
      </div>
      <div class="col text-center text-h2">{{ data.counter }}</div>
      <div class="col text-center">
        <q-btn
          v-touch-repeat:300:300:300:300:100.mouse="increaseCounter"
          icon="add"
          size="xl"
          round
          @click="increaseCounter"
        />
      </div>
    </div>
    <div class="row">
      <q-btn icon="restart_alt" size="xl" round @click="resetCounter" />
    </div>
  </q-page>
</template>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
}
</style>
