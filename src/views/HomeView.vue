<template>
  <div class="flex flex-col h-screen justify-between">
    <header class="flex justify-between py-4 px-20 bg-slate-200">
      <HeaderComponent
        @togglePopupEmit="togglePopup('add')"
        @searchEmit="search($event)"
      />
    </header>

    <main class="mb-auto p-4 overflow-auto">
      <TableComponent
        :employees="employees"
        @toggleSortEmit="toggleSort($event)"
        @togglePopupEmit="togglePopup('edit', $event)"
        @togglePopupDetailDelete="togglePopupDetailDelete($event)"
      />
    </main>

    <footer class="flex justify-center p-4 bg-slate-200">
      <FooterComponent
        :pageInfo="pageInfo"
        @showEmit="show($event)"
        @firstEmit="first"
        @prevEmit="prev"
        @lastEmit="last"
        @nextEmit="next"
      />
    </footer>
  </div>

  <AddEditPopupComponent
    v-if="showPopup"
    :popupType="popupType"
    :employee="employees[index]"
    @togglePopupEmit="togglePopup"
    @insertEmployeeEmit="insertEmployee($event)"
  />

  <DetailDeletePopupComponent
    v-if="showPopupDetailDelete"
    :popupType="popupType"
    :employee="employees[index]"
    @togglePopupEmit="togglePopupDetailDelete"
    @deleteEmployeeEmit="deleteEmployee($event)"
  />
</template>

<script>
import HeaderComponent from "@/components/HeaderComponent.vue";
import TableComponent from "@/components/TableComponent.vue";
import FooterComponent from "@/components/FooterComponent.vue";
import AddEditPopupComponent from "@/components/AddEditPopupComponent.vue";
import DetailDeletePopupComponent from "@/components/DetailDeletePopupComponent.vue";
import { mapGetters, mapActions } from "vuex";
import axios from "axios";

export default {
  components: {
    HeaderComponent,
    TableComponent,
    FooterComponent,
    AddEditPopupComponent,
    DetailDeletePopupComponent,
  },
  computed: mapGetters(["getData"]),
  data() {
    return {
      showPopup: false,
      showPopupDetailDelete: false,
      popupType: "add",
      index: 0,
      reqBody: {
        page: 1,
        limit: 3,
        sort: null,
        sort_direction: null,
        filter: null,
      },
      employees: [],
      pageInfo: {
        currentPage: 1,
        totalPages: 1,
      },
    };
  },
  methods: {
    ...mapActions(["fetchData"]),
    togglePopup(type, index) {
      this.popupType = type;
      this.index = index;
      this.showPopup = !this.showPopup;
    },
    togglePopupDetailDelete(indexAndType) {
      if (indexAndType !== undefined) {
        this.popupType = indexAndType.type;
        this.index = indexAndType.index;
      }
      this.showPopupDetailDelete = !this.showPopupDetailDelete;
    },
    async insertEmployee(employee) {
      if (employee.type === "add") {
        await axios.post("/add-employee", {
          name: employee.name,
          email: employee.email,
          mobile: employee.mobile,
          birthdate: employee.birthdate,
          address: employee.address,
        });
      }
      if (employee.type === "edit") {
        await axios.post("/edit-employee", {
          id: employee.id,
          name: employee.name,
          email: employee.email,
          mobile: employee.mobile,
          birthdate: employee.birthdate,
          address: employee.address,
        });
      }
      this.populateTable();
      this.togglePopup();
    },
    async deleteEmployee(id) {
      await axios.post("/delete-employee", { id: id });
      this.populateTable();
      this.togglePopupDetailDelete();
    },
    show(limit) {
      this.reqBody.limit = limit;
      this.reqBody.page = 1;
      this.pageInfo.currentPage = 1;
      this.populateTable();
    },
    search(keyword) {
      this.reqBody.filter = keyword;
      this.reqBody.page = 1;
      this.pageInfo.currentPage = 1;
      this.populateTable();
    },
    toggleSort(sort) {
      this.reqBody.sort = sort.type;
      this.reqBody.sort_direction = sort.direction;
      this.populateTable();
    },
    first() {
      this.reqBody.page = 1;
      this.pageInfo.currentPage = 1;
      this.populateTable();
    },
    prev() {
      this.pageInfo.currentPage = this.pageInfo.currentPage - 1;
      this.reqBody.page = this.pageInfo.currentPage;
      this.populateTable();
    },
    last() {
      this.reqBody.page = this.pageInfo.totalPages;
      this.pageInfo.currentPage = this.pageInfo.totalPages;
      this.populateTable();
    },
    next() {
      this.pageInfo.currentPage = this.pageInfo.currentPage + 1;
      this.reqBody.page = this.pageInfo.currentPage;
      this.populateTable();
    },
    async populateTable() {
      await this.fetchData({
        url: "/all-employee",
        body: this.reqBody,
      });
      this.employees = this.getData.data;
      this.pageInfo.totalPages = this.getData.message.total_pages;
    },
  },
  created() {
    this.populateTable();
  },
};
</script>

<style scoped></style>
