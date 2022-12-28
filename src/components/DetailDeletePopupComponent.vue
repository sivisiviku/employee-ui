<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="mb-4 flex justify-start">
        <h1 class="text-2xl">
          <span v-if="popupType === 'detail'">Detail</span
          ><span v-if="popupType === 'delete'">Delete</span> Employee
        </h1>
      </div>
      <div class="mb-4 grid grid-cols-6 gap-8">
        <div class="col-span-2 flex items-center">ID</div>
        <div disabled class="col-span-4 flex items-center">{{ id }}</div>
      </div>
      <hr />
      <div class="mb-4 grid grid-cols-6 gap-8">
        <div class="col-span-2 flex items-center">Name</div>
        <div class="col-span-4 flex items-center">{{ name }}</div>
      </div>
      <hr />
      <div class="mb-4 grid grid-cols-6 gap-8">
        <div class="col-span-2 flex items-center">Email</div>
        <div class="col-span-4 flex items-center">{{ email }}</div>
      </div>
      <hr />
      <div class="mb-4 grid grid-cols-6 gap-8">
        <div class="col-span-2 flex items-center">Mobile</div>
        <div class="col-span-4 flex items-center">{{ mobile }}</div>
      </div>
      <hr />
      <div class="mb-4 grid grid-cols-6 gap-8">
        <div class="col-span-2 flex items-center">Birthdate</div>
        <div class="col-span-4 flex items-center">{{ birthdate }}</div>
      </div>
      <hr />
      <div
        class="grid grid-cols-6 gap-8"
        v-for="(item, index) in address"
        :key="index"
      >
        <div class="col-span-2 flex items-center">
          <span v-if="index === 0"> Address </span>
        </div>
        <div class="col-span-4 flex items-center mb-2">
          <div>{{ address[index] }}</div>
        </div>
      </div>
      <hr />
      <div class="flex justify-start mt-8" v-if="popupType === 'delete'">
        Are you sure want to delete this employee?
      </div>
      <div class="flex justify-around mt-8">
        <button
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="togglePopupEmit"
        >
          <span v-if="popupType === 'detail'">Close</span>
          <span v-if="popupType === 'delete'">Cancel</span>
        </button>
        <button
          v-if="popupType === 'delete'"
          class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="deleteEmployeeEmit"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  props: ["popupType", "employee"],
  data() {
    return {
      id: 0,
      name: "",
      email: "",
      mobile: "",
      birthdate: "",
      address: [""],
    };
  },
  methods: {
    togglePopupEmit() {
      this.$emit("togglePopupEmit");
    },
    deleteEmployeeEmit() {
      this.$emit("deleteEmployeeEmit", this.id);
    },
  },
  created() {
    if (this.employee !== undefined) {
      this.id = this.employee.id;
      this.name = this.employee.name;
      this.email = this.employee.email;
      this.mobile = this.employee.mobile;
      this.birthdate = moment(this.employee.birthdate).format("MMMM Do, YYYY");
      this.address = JSON.parse(this.employee.address);
    }
  },
};
</script>

<style scoped>
.popup {
  background-color: rgba(0, 0, 0, 0.2);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-inner {
  background: #fff;
  padding: 32px;
  border-radius: 10px;
}
</style>
