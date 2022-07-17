<template>
    <v-card class="mx-auto my-6" max-width="374">
        <v-img
            height="250"
            src="https://cdn.vuetifyjs.com/images/cards/cooking.png"
        ></v-img>

        <v-card-title>{{body.title}}</v-card-title>

        <v-card-text>
            <div>Цена: {{body.price}} $</div>
        </v-card-text>
        <v-card-actions>
            <v-btn
                color="deep-purple lighten-2"
                text
            >
                Open
            </v-btn>
        </v-card-actions>
    </v-card>
</template>
<script>
export default {
    name: 'Advertisement',
    props: ['body']
}
</script>
<style lang="">
    
</style>