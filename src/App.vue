<script setup>
import { ref, computed } from "vue";
import AddOrganizationModal from "./components/AddOrganizationModal.vue";
import OrganizationTable from "./components/OrganizationTable.vue";

const showModal = ref(false);
const organizations = ref([]);
const director = ref("");

const onClickAdd = () => {
  showModal.value = !showModal.value;
};
const onClickAddItem = (item) => {
  organizations.value.push(item);
};

const removeOrganization = (organization) => {
  organizations.value = organizations.value.filter((obj) => obj !== organization);
};

const findOrganizationByName = computed(() => {
  if (director.value === "") return organizations.value;
  else {
    return [...organizations.value].filter((obj) =>
      obj.director.includes(director.value)
    );
  }
});
</script>

<template>
  <AddOrganizationModal
    v-if="showModal"
    :onClickAdd="onClickAdd"
    :onClickAddItem="onClickAddItem"
    :organizations="organizations"
  />
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">Справочник организаций</h1>
    <div class="flex justify-between items-center">
      <input
        v-model="director"
        placeholder="Поиск..."
        class="border rounded py-1 px-4 border-blue-500 outline-none"
        type="text"
      />
      <button
        class="bg-blue-500 text-white py-2 px-4 rounded mb-4"
        @click="onClickAdd"
      >
        Добавить
      </button>
    </div>

    <OrganizationTable
      :organizations="findOrganizationByName"
      :removeOrganization="removeOrganization"
    />
  </div>
</template>

<style scoped></style>
