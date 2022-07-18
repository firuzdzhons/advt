<template lang="">
    <v-row v-if="advertisement">
        <v-col cols="12" sm="8" md="6">
            <v-img
                lazy-src="https://picsum.photos/id/11/10/6"
                max-height="600"
                max-width="600"
                src="https://picsum.photos/id/11/500/300"
            ></v-img>
        </v-col>
        <v-col cols="12" sm="8" md="6">
            <p><span class="font-weight-bold">Название:</span> {{advertisement.title}}</p>
            <p><span class="font-weight-bold">Цена:</span> {{advertisement.price}}</p>
            <p class="font-weight-bold">Описание:</p>
            <p>{{advertisement.description}}</p>
        </v-col>
    </v-row>
</template>
<script>
export default {
    name: 'ShowAdvt',
    data() {
        return {
            advertisement: null
        }
    },
    mounted() {
        this.fetchAdvtById(this.$route.params.id);
    },

    methods: {
        fetchAdvtById(id) {
            this.$axios.get(`advertisement/${id}`)
            .then(({data}) => {
                this.advertisement = data
            })
            .catch(function (error) {
                console.log(error);
            }).finally(() => {
                this.loading = false;
            });
        }
    },
}
</script>
<style lang="">
    
</style>