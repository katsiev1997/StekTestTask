<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  organizations: Array,
  removeOrganization: Function,
});

const page = ref(1);
const sortField = ref("name");
const sortAscending = ref(true);
const itemsPerPage = 3;

const pages = computed(() => {
  return Math.ceil(props.organizations.length / itemsPerPage);
});

const sortedOrganizations = computed(() => {
  const start = (page.value - 1) * itemsPerPage;
  return [...props.organizations]
    .sort((a, b) => {
      const fieldA = a[sortField.value].toLowerCase();
      const fieldB = b[sortField.value].toLowerCase();

      if (fieldA < fieldB) return sortAscending.value ? -1 : 1;
      if (fieldA > fieldB) return sortAscending.value ? 1 : -1;
      return 0;
    })
    .slice(start, start + itemsPerPage);
});

const incrementPage = () => {
  if (page.value < pages.value) {
    page.value++;
  }
};

const decrementPage = () => {
  if (page.value > 1) {
    page.value--;
    console.log(page.value);
    console.log(pages.value);
  }
};

const selectPage = (pageNumber) => {
  page.value = pageNumber;
};

const sortOrganizations = (field) => {
  if (sortField.value === field) {
    sortAscending.value = !sortAscending.value;
  } else {
    sortField.value = field;
    sortAscending.value = true;
  }
  // Сброс на первую страницу при смене сортировки
  page.value = 1;
};
</script>

<template>
  <div>
    <table class="min-w-full bg-white border border-gray-200">
      <thead class="bg-gray-100 border-b">
        <tr>
          <th class="py-2 px-4 text-left">
            <button @click="sortOrganizations('name')" class="text-blue-500">
              Название организации
            </button>
          </th>
          <th class="py-2 px-4 text-left">
            <button @click="sortOrganizations('director')" class="text-blue-500">
              ФИО директора
            </button>
          </th>
          <th class="py-2 px-4 text-left">Номер телефона</th>
          <th class="py-2 px-4 text-left">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="organization in sortedOrganizations" :key="organization.name + organization.director + organization.phone" class="border-b">
          <td class="py-2 px-4">{{ organization.name }}</td>
          <td class="py-2 px-4">{{ organization.director }}</td>
          <td class="py-2 px-4">{{ organization.phone }}</td>
          <td class="py-2 px-4">
            <button @click="removeOrganization(organization)" class="text-red-500">
              x
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="flex justify-center mt-4">
      <button
        @click="decrementPage"
        :disabled="page === 1"
        class="px-3 py-1 border border-gray-300 rounded-l disabled:text-gray-400"
      >
        Previous
      </button>
      <button
        v-for="pageNumber in pages"
        :key="pageNumber"
        @click="selectPage(pageNumber)"
        :class="{
          'px-3 py-1 border border-gray-300': true,
          'text-blue-500': pageNumber === page,
        }"
        :disabled="pageNumber === page"
      >
        {{ pageNumber }}
      </button>
      <button
        @click="incrementPage"
        :disabled="page === pages || pages === 0"
        class="px-3 py-1 border border-gray-300 rounded-r disabled:text-gray-400"
      >
        Next
      </button>
    </div>
  </div>
</template>
