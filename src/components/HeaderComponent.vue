<template>
  <h1 class="text-2xl font-semibold text-slate-600">List Employee</h1>
  <div class="flex">
    <input
      class="shadow appearance-none border rounded-l w-60 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      id="username"
      type="text"
      placeholder="Search by ID, name, or email"
      v-model="keyword"
    />
    <button
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-3 rounded-r focus:outline-none focus:shadow-outline mr-4"
      type="button"
      @click="searchEmit"
    >
      Search
    </button>
    <button
      class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-3 rounded focus:outline-none focus:shadow-outline"
      type="button"
      @click="togglePopupEmit"
    >
      Add
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    togglePopupEmit() {
      this.$emit("togglePopupEmit");
    },
    searchEmit() {
      this.$emit("searchEmit", this.keyword);
    },
  },
};
</script>

<style lang="scss" scoped></style>
