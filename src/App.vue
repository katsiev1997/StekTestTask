<script setup>
import { ref } from "vue";
import AddOrganizationModal from "./components/AddOrganizationModal.vue";
import OrganizationTable from "./components/OrganizationTable.vue";

const showModal = ref(false);
const organizations = ref([]);

const onClickAdd = () => {
  showModal.value = !showModal.value;
};
const onClickAddItem = (item) => {
  organizations.value.push(item);
};

const removeOrganization = (organization) => {
  organizations.value = organizations.value.filter((obj) => obj !== organization);
};
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
    <button
      class="bg-blue-500 text-white py-2 px-4 rounded mb-4"
      @click="onClickAdd"
    >
      Добавить
    </button>
    <OrganizationTable
      :organizations="organizations"
      :removeOrganization="removeOrganization"
    />
  </div>
</template>

<style scoped></style>
