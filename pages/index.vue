<template>
  <div>
    <h1 class="my-5">Все объявления</h1>
    <div class="text-center font-weight-bold" v-if="loading">
       Идет загрузка ...
    </div>
    <div class="d-inline-flex">
         <v-select
            filled
            :items="sortColumns"
            label="Сортировать по"
            item-text="text"
            item-value="value"
            class="mr-4"
            v-model="sortByColumn"
            @change="sortAdvt"
        ></v-select>
        <v-select
            filled
            :items="sortDirections"
            item-text="text"
            item-value="value"
            label="Направление сортировки"
            v-model="sortDirection"
            @change="sortAdvt"
        ></v-select>
    </div>
    <v-row align="center" v-if="advertisements.length">
      <v-col cols="12" sm="6" md="3" v-for="advt in advertisements" :key="advt.id">
        <Advertisement :body="advt"/>
      </v-col>
    </v-row>
  <div class="text-center" v-if="advertisements.length">
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

      this.length = parseInt(response.meta.last_page);

      this.$router.push({ query: { 
          page: response.meta.current_page,
          sortByColumn: this.sortByColumn,
          sortDirection: this.sortDirection
        }})

      this.loading = false;
    },

    sortAdvt() {
      if(this.sortByColumn && this.sortDirection) {
        this.fetchAdvertisements()
      }

    }
  },
}
</script>
