<template>
  Show
  <select class="mx-2 rounded w-14" name="" id="" @change="showEmit($event)">
    <option value="3">3</option>
    <option value="6">6</option>
    <option value="9">9</option>
  </select>
  per page
  <button
    class="ml-4"
    type="button"
    :disabled="pageInfo.currentPage === 1"
    @click="firstEmit"
  >
    &lt;&lt;
  </button>
  <button
    class="mx-2"
    type="button"
    :disabled="pageInfo.currentPage === 1"
    @click="prevEmit"
  >
    &lt;
  </button>
  page {{ pageInfo.currentPage }} of {{ pageInfo.totalPages }}
  <button
    class="ml-2"
    type="button"
    :disabled="pageInfo.currentPage === pageInfo.totalPages"
    @click="nextEmit"
  >
    >
  </button>
  <button
    class="ml-2"
    type="button"
    :disabled="pageInfo.currentPage === pageInfo.totalPages"
    @click="lastEmit"
  >
    >>
  </button>
</template>

<script>
export default {
  props: ["pageInfo"],
  methods: {
    showEmit(event) {
      this.$emit("showEmit", event.target.value);
    },
    firstEmit() {
      this.$emit("firstEmit");
    },
    prevEmit() {
      this.$emit("prevEmit");
    },
    lastEmit() {
      this.$emit("lastEmit");
    },
    nextEmit() {
      this.$emit("nextEmit");
    },
  },
};
</script>

<style lang="scss" scoped></style>
