<template>
  <form id="app" @submit="checkForm" method="post">
      <h1>Форма обратной связи</h1>
      <div>
          <label for="name">Имя:</label>
          <input type="text" id="name" name="name" v-model="form.name" :error-messages="nameErrors" >
      </div>
      <div>
          <label for="email">Email:</label>
          <input type="email" id="email" name="email" v-model="form.email" >
      </div>
      <div>
          <label for="message">Сообщение:</label>
          <textarea id="message" name="message" v-model="form.message" ></textarea>
      </div>
      <button type="submit" >Отправить</button>
    </form>
</template>

<script>
// import { validationMixin } from 'vuelidate';
import axios from 'axios';
import { required, email } from 'vuelidate/lib/validators'

export default {
  // mixins:  [ validationMixin, ],
  data() {
    return {
      form:{
        name: '',
        email: '',
        message: ''
      }
    }
  },
  validations:{
    form:{
        name: {required},
        email: {required,email},
        message: {required},
      }
  },
  // computed:{
  //   nameErrors(){
  //     const errors = [];
  //     if (!this.form.name.required) errors.push('Обязательное поле для заполнения.')
  //     return errors;
  //   }
  // },
  methods: {
    checkForm() {
      console.log('Form data:', this.form); // отладочная информация
      axios.post('index.php', this.form)
        .then(response => {
          this.successMessage = response.data;
        })
        .catch(error => {
          this.errorMessage = error.response.data;
        });
    }
  }
}
</script>
