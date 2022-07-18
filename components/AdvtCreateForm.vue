<template lang="">
  <v-card class="mx-auto my-12 pa-9">
      <h2 class="text-center">
        Создание объявлений
      </h2>
     <v-alert
          dense
          text
          type="success"
          v-if="successResponse"
        >
          {{successResponse.message}}
      </v-alert>
      <v-form ref="form" v-model="valid" lazy-validation>
        
        <v-text-field
          v-model="title"
          :rules="requiredRule"
          label="Название*"
          required
        ></v-text-field>
        <v-text-field
          v-model="price"
          label="Цена*"
          :rules="requiredRule"
          type="number"
          min="0"
          required
        ></v-text-field>
        <div>
        </div>
        <div>
            <p class="font-weight-bold mt-5">Ссылки на изображения*</p>
            <div v-for="(link, index) in imageLinks" class="d-flex">
              <v-text-field
                v-model="link.value"
                :rules="requiredRule"
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
              color="primary"
              @click="addImageLink"
              :disabled="imageLinks.length == 3"
            >
            Добавить ссылку
          </v-btn>
          
        </div>
        <v-textarea
          name="description"
          label="Описание"
          v-model="description"
          :rules="requiredRule"
        ></v-textarea>

        <v-btn
          @click="createAdvt"
          :loading="loading"
          depressed
          color="primary"
        >
          Создать
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
      successResponse: '', 
      requiredRule: [
        v => !!v || 'Поле, обязательное для заполнения',
      ],
    }),

    methods: {
      createAdvt(){
        this.loading = true;
        let imageLinks = [];
        this.successResponse = null;


        for (let i in this.imageLinks) {
            imageLinks.push(this.imageLinks[i].value)
        }
        this.$axios.post('advertisement', {
          'title': this.title,
          'description': this.description,
          'price': this.price,
          'image_links': imageLinks
        })
            .then(({data}) => {
              this.successResponse = data;
              this.reset();
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