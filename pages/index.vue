<template>
  <div>
    <h1>All advertisements</h1>
    <div class="d-inline-flex">
         <v-select
            filled
            :items="sortColumns"
            label="Сортировать по"
            item-text="text"
            item-value="value"
            class="mr-4"
            v-model="sortByColumn"
            @change="fetchAdvertisements"
        ></v-select>
        <v-select
            filled
            :items="sortDirections"
            label="Направление сортировки"
            v-model="sortDirection"
            @change="fetchAdvertisements"
        ></v-select>
    </div>
    <v-row align="center">
      <v-col cols="12" sm="6" md="3" v-for="advt in advertisements" :key="advt.id">
        <Advertisement :body="advt"/>
      </v-col>
  </v-row>
  <div class="text-center">
      <v-pagination
          v-model="page"
          :length="length"
          circle
          @input="fetchAdvertisements"
          :loading = "loading"
      ></v-pagination>
    </div>
  </div>

</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      page: 1,
      length: 0,
      advertisements: [],
      loading: false,
      sortByColumn: '',
      sortDirection: '',
      sortColumns: [
        {"value": "price", "text": "Цена"},
        {"value": "created_at", "text": "Дата создания"}
      ],
      sortDirections: [
        {"value": "asc", "text": "По возрастанию"},
        {"value": "desc", "text": "По убыванию"}
      ]

    }
  },
  created() {
    this.init();
    this.fetchAdvertisements();
  },
  methods: {
    init() {
        this.sortByColumn = this.$route.query.sortByColumn;

        this.sortDirection = this.$route.query.sortDirection;

        if (this.$route.query.page) {
            this.page = parseInt(this.$route.query.page);
        } else {
            this.$router.push({ query: { page: this.page }})
    }
    },

    async fetchAdvertisements () {
      this.loading  = true;

      const response = (await this.$axios.$get(`advertisement?page=${this.page}&sort_by_column=${this.sortByColumn}&sort_direction=${this.sortDirection}`))

      this.advertisements = response.data;

      this.length = parseInt(response.last_page);

      this.$router.push({ query: { 
          page: response.current_page,
          sortByColumn: this.sortByColumn,
          sortDirection: this.sortDirection
        }})

      this.loading = false;
    },
  },
}
</script>
