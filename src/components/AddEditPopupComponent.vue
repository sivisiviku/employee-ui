<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="mb-4 flex justify-start">
        <h1 class="text-2xl">
          <span v-if="popupType === 'add'">Add</span
          ><span v-if="popupType === 'edit'">Edit</span> Employee
        </h1>
      </div>
      <div class="mb-4 grid grid-cols-6" v-if="popupType === 'edit'">
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="name">ID</label>
        </div>
        <input
          disabled
          class="col-span-4 shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="name"
          type="text"
          v-model="id"
        />
      </div>
      <div class="mb-4 grid grid-cols-6">
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="name">Name</label>
        </div>
        <div class="col-span-4 text-left">
          <input
            class="w-full shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            :class="{
              'border-2': validation.name === false,
              'border-red-600': validation.name === false,
            }"
            id="name"
            type="text"
            v-model="name"
            @change="checkValidation('name')"
          />
          <div class="text-sm text-red-600" v-if="validation.name === false">
            Name cannot be empty
          </div>
        </div>
      </div>
      <div class="mb-4 grid grid-cols-6">
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="email">Email</label>
        </div>
        <div class="col-span-4 text-left">
          <input
            class="w-full shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            :class="{
              'border-2': validation.email === false,
              'border-red-600': validation.email === false,
            }"
            id="email"
            type="email"
            v-model="email"
            @change="checkValidation('email')"
          />
          <div class="text-sm text-red-600" v-if="validation.email === false">
            Invalid email format
          </div>
        </div>
      </div>
      <div class="mb-4 grid grid-cols-6">
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="mobile">Mobile</label>
        </div>
        <div class="col-span-4 text-left">
          <input
            class="w-full shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            :class="{
              'border-2': validation.mobile === false,
              'border-red-600': validation.mobile === false,
            }"
            id="mobile"
            type="number"
            v-model="mobile"
            @change="checkValidation('mobile')"
          />
          <div class="text-sm text-red-600" v-if="validation.mobile === false">
            Invalid mobile format
          </div>
        </div>
      </div>
      <div class="mb-4 grid grid-cols-6">
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="birthdate">Birthdate</label>
        </div>
        <div class="col-span-4 text-left">
          <input
            type="date"
            class="w-full shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            :class="{
              'border-2': validation.birthdate === false,
              'border-red-600': validation.birthdate === false,
            }"
            id="birthdate"
            v-model="birthdate"
            @change="checkValidation('birthdate')"
          />
          <div
            class="text-sm text-red-600"
            v-if="validation.birthdate === false"
          >
            Please enter your birthdate
          </div>
        </div>
      </div>
      <div
        class="grid grid-cols-6"
        v-for="(item, index) in address"
        :key="index"
      >
        <div class="col-span-2 flex items-center">
          <label class="text-gray-700" for="address" v-if="index === 0">
            Address
          </label>
        </div>
        <div class="col-span-4 text-left">
          <div class="flex justify-between mb-2">
            <input
              class="w-full shadow appearance-none border rounded-l py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              :class="{
                'border-2':
                  validation.address === false && address[index] === '',
                'border-red-600':
                  validation.address === false && address[index] === '',
              }"
              id="address"
              type="text"
              v-model="address[index]"
              @change="checkValidation('address')"
            />
            <button
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-r focus:outline-none focus:shadow-outline"
              type="button"
              v-if="index === address.length - 1"
              @click="addAddress"
            >
              +
            </button>
            <button
              class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-r focus:outline-none focus:shadow-outline"
              type="button"
              v-if="index !== address.length - 1"
              @click="removeAddress(index)"
            >
              -
            </button>
          </div>
          <div
            class="text-sm text-red-600"
            v-if="validation.address === false && address[index] === ''"
          >
            Address cannot be empty
          </div>
        </div>
      </div>
      <div class="flex justify-around mt-8">
        <button
          class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="togglePopupEmit"
        >
          Cancel
        </button>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="insertEmployeeEmit"
        >
          Submit
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
      validation: {
        name: true,
        email: true,
        mobile: true,
        birthdate: true,
        address: true,
      },
    };
  },
  methods: {
    addAddress() {
      this.address.push("");
    },
    removeAddress(index) {
      this.address.splice(index, 1);
    },
    togglePopupEmit() {
      this.$emit("togglePopupEmit");
    },
    insertEmployeeEmit() {
      if (this.checkValidation("all") === false) return;
      this.$emit("insertEmployeeEmit", {
        type: this.popupType,
        id: this.id,
        name: this.name,
        email: this.email,
        mobile: this.mobile,
        birthdate: this.birthdate,
        address: this.address,
      });
      this.name = "";
      this.email = "";
      this.mobile = "";
      this.birthdate = "";
      this.address = [""];
    },
    checkValidation(type) {
      const validateEmail = (email) => {
        return String(email)
          .toLowerCase()
          .match(
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          );
      };
      const validateMobile = (mobile) => {
        return String(mobile).match(/^\d+$/);
      };
      if (type === "name" || type === "all") {
        if (this.name === "") {
          this.validation.name = false;
        } else {
          this.validation.name = true;
        }
      }
      if (type === "email" || type === "all") {
        if (this.email === "" || !validateEmail(this.email)) {
          this.validation.email = false;
        } else {
          this.validation.email = true;
        }
      }
      if (type === "mobile" || type === "all") {
        if (this.mobile === "" || !validateMobile(this.mobile)) {
          this.validation.mobile = false;
        } else {
          this.validation.mobile = true;
        }
      }
      if (type === "birthdate" || type === "all") {
        if (this.birthdate === "") {
          this.validation.birthdate = false;
        } else {
          this.validation.birthdate = true;
        }
      }
      if (type === "address" || type === "all") {
        if (this.address.includes("")) {
          this.validation.address = false;
        } else {
          this.validation.address = true;
        }
      }
      for (let key in this.validation) {
        if (this.validation[key] === false) return false;
      }
      return true;
    },
  },
  created() {
    if (this.employee !== undefined) {
      this.id = this.employee.id;
      this.name = this.employee.name;
      this.email = this.employee.email;
      this.mobile = this.employee.mobile;
      this.birthdate = `${moment(this.employee.birthdate).year()}-${
        moment(this.employee.birthdate).month() + 1
      }-${moment(this.employee.birthdate).date()}`;
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
