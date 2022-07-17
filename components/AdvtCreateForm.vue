<template lang="">
  <v-card class="mx-auto my-12 pa-9">
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-text-field
          v-model="title"
          :rules="titleRules"
          label="Title"
          required
        ></v-text-field>
        <v-text-field
          v-model="price"
          label="Price"
          type="number"
          min="0"
          required
        ></v-text-field>
        <div>
        </div>
        <div>
            <p>Image links</p>
            <div v-for="(link, index) in imageLinks" class="d-flex">
              <v-text-field
                v-model="link.value"
                required
              ></v-text-field>
              <v-btn
                class="mx-2"
                icon
                small
                color="red"
                v-if="index > 0"
                @click="removeImageLink(link)"
              >
              <v-icon dark>
                mdi-minus
              </v-icon>
            </v-btn>
            </div>
            <v-btn
              text
              color="primary"
              @click="addImageLink"
              :disabled="imageLinks.length == 3"
            >
            Add link
          </v-btn>
          
        </div>
        <v-textarea
          name="description"
          label="Description"
          v-model="description"
        ></v-textarea>

        <v-btn
          @click="createAdvt"
          :loading="loading"
          depressed
          color="primary"
        >
          Create
        </v-btn>
    </v-form>
  </v-card>
    
</template>
<script>
export default {
    name: 'AdvtCreateForm',
  
    data: () => ({
      valid: true,
      loading: false,
      title: '',
      price: '',
      imageLinks: [
        {value: ''}
      ],
      description: '',
      titleRules: [
        v => !!v || 'Title is required',
      ],
    }),

    methods: {
      createAdvt(){
        this.loading = true;
        let imageLinks = [];


        for (let i in this.imageLinks) {
            imageLinks.push(this.imageLinks[i].value)
        }
        this.$axios.post('advertisement', {
          'title': this.title,
          'description': this.description,
          'price': this.price,
          'image_links': imageLinks
        })
            .then(() => {
                this.reset()
            })
            .catch(function (error) {
                console.log(error);
            }).finally(() => {
                this.loading = false;
            });
      },
      addImageLink() {
        this.imageLinks.push({ value: "" });
      },
      removeImageLink(link) {
        const index = this.imageLinks.indexOf(link);
        if (index > -1) {
          this.imageLinks.splice(index, 1);
        }
      },
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.imageLinks = []
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
    },
}
</script>
<style lang="">
    
</style>