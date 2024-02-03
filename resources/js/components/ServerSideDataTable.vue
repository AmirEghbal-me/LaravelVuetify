<template>
  <v-data-table-server
      v-model:items-per-page="itemsPerPage"
      :headers="headers"
      :items-length="totalItems"
      :items="serverItems"
      :loading="loading"
      :search="search"
      item-value="name"
      :items-per-page-text= '$t("items-per-page-text")'
      @update:options="loadItems"
  >

      <template #top>
          <v-toolbar flat>
              <v-toolbar-title class="text-h5">لیست کاربران لاراول</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn text @click="refreshData">به روز رسانی</v-btn>
          </v-toolbar>
      </template>
      <template v-slot:tfoot>
          <tr>
              <td>
                  <v-text-field v-model="name" hide-details placeholder="جستجوی نام..." class="ma-2" density="compact"></v-text-field>
              </td>
          </tr>
      </template>
      <template v-slot:item.actions="{ item }">
          <v-icon
              size="small"
              class="me-2"
              @click="editItem(item)"
          >
              mdi-pencil
          </v-icon>
          <v-icon
              size="small"
              @click="deleteItem(item)"
          >
              mdi-delete
          </v-icon>
      </template>
      <template v-slot:no-data>
          <v-btn
              color="primary"
              @click="initialize"
          >
              Reset
          </v-btn>
      </template>
  </v-data-table-server>
</template>
<script>
import axios from 'axios';
export default {
  data: () => ({
    itemsPerPage: 5,
    headers: [
      { title: 'شناسه', key: 'id', align: 'end' },
      { title: 'نام', key: 'name', align: 'end' },
      { title: 'ایمیل', key: 'email', align: 'end' },
      { title: 'عملیات', value: 'actions', sortable: false, align: 'end' },
    ],
    search: '',
    serverItems: [],
    loading: true,
    name : '',
    totalItems: 0,
  }),
    watch: {
        name () {
            this.search = String(Date.now())
        },
    },
  methods: {
    loadItems ({ page = null, itemsPerPage = null }) {
      this.loading = true
      const start = (page - 1) * itemsPerPage
      const end = start + itemsPerPage
      axios.post(`http://localhost:8000/api/users?page=${page}&perPage=${itemsPerPage}&search=${this.name}`)
          .then(response => {
            this.serverItems = response.data.data;
            this.totalItems = response.data.total;
            this.loading = false;
          })
          .catch(error => {
            console.log(error);
            this.loading = false;
          });
    },
      handleAction(item) {
          // Implement your action logic here
          console.log('Performing action on item:', item);
          console.log('Name:', this.name);
      },
      async refreshData() {
          await this.loadItems({});
      },
  },
}
</script>
