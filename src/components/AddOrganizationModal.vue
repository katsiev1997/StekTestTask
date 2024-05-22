<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  onClickAdd: Function,
  onClickAddItem: Function,
  organizations: Array,
});

const name = ref("");
const director = ref("");
const phone = ref("");

const onClickOk = () => {
  props.onClickAddItem({
    name: name.value,
    director: director.value,
    phone: phone.value,
  });
  name.value = "";
  director.value = "";
  phone.value = "";
};

const isButtonDisabled = computed(() => {
  return name.value === "" || director.value === "" || phone.value === "";
});
</script>

<template>
  <div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
    <div class="bg-white rounded p-4 w-full max-w-md">
      <div class="flex justify-between items-center border-b pb-2 mb-4">
        <h5 class="text-xl">Добавить организацию</h5>
        <button @click="props.onClickAdd" class="text-gray-500">x</button>
      </div>
      <div class="mb-4">
        <label for="name" class="block text-sm font-medium mb-1"
          >Название организации</label
        >
        <input
          v-model="name"
          type="text"
          id="name"
          class="border p-2 w-full"
        />
      </div>
      <div class="mb-4">
        <label for="director" class="block text-sm font-medium mb-1"
          >ФИО директора</label
        >
        <input
          v-model="director"
          type="text"
          id="director"
          class="border p-2 w-full"
        />
      </div>
      <div class="mb-4">
        <label for="phone" class="block text-sm font-medium mb-1"
          >Номер телефона</label
        >
        <input v-model="phone" type="text" id="phone" class="border p-2 w-full" />
      </div>
      <div class="flex justify-end">
        <button
          @click="props.onClickAdd"
          class="bg-gray-500 text-white py-2 px-4 rounded mr-2"
        >
          Отмена
        </button>
        <button
          :disabled="isButtonDisabled"
          @click="onClickOk"
          class="bg-blue-500 text-white py-2 px-4 rounded disabled:bg-slate-600"
        >
          ОК
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
