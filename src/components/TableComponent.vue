<template>
  <table class="mx-auto">
    <thead>
      <tr>
        <th class="border-2 p-2" style="width: 10%">
          ID
          <button
            class="float-right"
            type="button"
            v-if="sort.id === 'asc'"
            @click="toggleSortEmit('id', 'desc')"
          >
            🔽
          </button>
          <button
            class="float-right"
            type="button"
            v-if="sort.id === 'desc'"
            @click="toggleSortEmit('id', 'asc')"
          >
            🔼
          </button>
        </th>
        <th class="border-2 p-2" style="width: 30%">
          Name
          <button
            class="float-right"
            type="button"
            v-if="sort.name === 'asc'"
            @click="toggleSortEmit('name', 'desc')"
          >
            🔽
          </button>
          <button
            class="float-right"
            type="button"
            v-if="sort.name === 'desc'"
            @click="toggleSortEmit('name', 'asc')"
          >
            🔼
          </button>
        </th>
        <th class="border-2 p-2" style="width: 30%">
          Email
          <button
            class="float-right"
            type="button"
            v-if="sort.email === 'asc'"
            @click="toggleSortEmit('email', 'desc')"
          >
            🔽
          </button>
          <button
            class="float-right"
            type="button"
            v-if="sort.email === 'desc'"
            @click="toggleSortEmit('email', 'asc')"
          >
            🔼
          </button>
        </th>
        <th class="border-2 p-2" style="width: 15%">
          Mobile
          <button
            class="float-right"
            type="button"
            v-if="sort.mobile === 'asc'"
            @click="toggleSortEmit('mobile', 'desc')"
          >
            🔽
          </button>
          <button
            class="float-right"
            type="button"
            v-if="sort.mobile === 'desc'"
            @click="toggleSortEmit('mobile', 'asc')"
          >
            🔼
          </button>
        </th>
        <th class="border-2 p-2" style="width: 15%">Action</th>
      </tr>
    </thead>
    <tbody v-if="employees.length > 0">
      <tr v-for="(employee, index) in employees" :key="index">
        <td class="border-2 p-2">{{ employee.id }}</td>
        <td class="border-2 p-2">{{ employee.name }}</td>
        <td class="border-2 p-2">{{ employee.email }}</td>
        <td class="border-2 p-2">{{ employee.mobile }}</td>
        <td class="border-2 p-2">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-2 rounded focus:outline-none focus:shadow-outline mr-2"
            type="button"
            @click="togglePopupDetailDelete({ index: index, type: 'detail' })"
          >
            Detail
          </button>
          <button
            class="bg-green-500 hover:bg-green-700 text-white font-bold py-1 px-2 rounded focus:outline-none focus:shadow-outline mr-2"
            type="button"
            @click="togglePopupEmit(index)"
          >
            Edit
          </button>
          <button
            class="bg-red-500 hover:bg-red-700 text-white font-bold py-1 px-2 rounded focus:outline-none focus:shadow-outline"
            type="button"
            @click="togglePopupDetailDelete({ index: index, type: 'delete' })"
          >
            Delete
          </button>
        </td>
      </tr>
    </tbody>
  </table>
  <div class="mt-8 flex justify-center" v-if="employees.length === 0">
    List empty
  </div>
</template>

<script>
export default {
  props: ["employees"],
  data() {
    return {
      sort: {
        id: "asc",
        name: "asc",
        email: "asc",
        mobile: "asc",
      },
    };
  },
  methods: {
    toggleSortEmit(type, direction) {
      this.sort[`${type}`] = direction;
      this.$emit("toggleSortEmit", {
        type: type,
        direction: direction,
      });
    },
    togglePopupEmit(index) {
      this.$emit("togglePopupEmit", index);
    },
    togglePopupDetailDelete(indexAndType) {
      this.$emit("togglePopupDetailDelete", indexAndType);
    },
  },
};
</script>

<style lang="scss" scoped></style>
