<template>
  <div class="user-component">
    <div class="user-form q-mb-md">
      <q-card class="my-card" style="overflow: auto">
        <q-card-section>
          <h5>{{ title }}</h5>
          <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
            <div class="row justify-between">
              <div class="col-md-3 q-pa-xs">
                <q-input
                  filled
                  v-model="name"
                  label="نام *"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'فیلد نام اجباری است',
                  ]"
                />
              </div>
              <div class="col-md-3 q-pa-xs">
                <q-input
                  filled
                  type="text"
                  v-model="userName"
                  label="نام کاربری  *"
                  lazy-rules
                  :rules="[
                    (val) =>
                      (val && val.length > 0) || 'فیلد نام کاربری اجباری است',
                  ]"
                />
              </div>
              <div class="col-md-3 q-pa-xs">
                <q-input
                  filled
                  v-model="email"
                  label="ایمیل *"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'فیلد ایمیل اجباری است',
                  ]"
                />
              </div>
              <div class="col-md-3 q-pa-xs">
                <q-input filled v-model="address" label="آدرس *" lazy-rules />
              </div>
            </div>

            <!-- <div class="col-md-3"><q-toggle v-model="accept" label="I accept the license and terms" /></div> -->

            <div class="float-right q-mb-md">
              <q-btn
                label="ثبت"
                type="submit"
                color="secondary"
                class="text-white"
              />
              <q-btn
                label="پاک کردن"
                type="reset"
                color="primary"
                flat
                class="q-ml-sm"
              />
            </div>
          </q-form> </q-card-section
      ></q-card>
    </div>

    <!-- shayan-table -->
    <t-table
      :columns.sync="columns"
      :rows.sync="rows"
      title="لیست کاربران"
      @edit-click="editRow"
      @delete-click="deleteRow"
      @access-click="accessRow"
      :actions="actions"
    />
    <!----------->
  </div>
</template>

<script>
import tTable from "shayan-ui-framework/src/components/Table.vue";
import { ref } from "vue";
export default {
  name: "user",
  components: { tTable },
  data() {
    return {
      columns: [
        {
          name: "id",
          classes: "hidden",
          headerClasses: "hidden",
        },
        { name: "lastName", label: "نام" },
        { name: "userName", label: "نام کاربری" },
        { name: "email", label: "ایمیل" },
        { name: "actions", align: "center", label: "عملیات" },
      ],
      rows: [
        {
          id: "1",
          lastName: "مریم رستمی",
          userName: "m.rostami",
          email: "maryamrostmai598@gmail.com",
        },
        {
          id: "2",
          lastName: " سپیده رضایی",
          userName: "s.rezaei",
          email: "sepide.rezaei@gmail.com",
        },
      ],
      actions: [
        {
          name: "access",
          label: "دسترسی",
          color: "secondary",
          event: this.accessRow,
        },
        { name: "edit", label: "ویرایش", color: "amber", event: this.editRow },
        { name: "delete", label: "حذف", color: "red", event: this.deleteRow },
      ],
    };
  },
  setup() {
    return {
      confirm: ref(false),
    };
  },
  created() {
    this.$store.dispatch("fetchUserList");
    console.log("created called.");
  },

  methods: {
    selectedRow: (evt, row, index) => {
      console.log("selected row ::::", evt, row.id, index);
    },
    accessRow(props) {
      console.log("access row ::::", props.row);
    },
    editRow(props) {
      console.log("Edit row ::::", props.row);
    },
    deleteRow(props) {
      console.log("Delete row ::::", props.row.id);
    },
  },
  computed: {
    // Get From Store
    userList() {
      return this.$store.getters.userList;
    },
  },
};
</script>
