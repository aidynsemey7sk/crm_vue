<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Регистрация</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Почта</label>
                        <div class="control">
                            <input type="email" name="email" class="input" v-model="email">
                        </div>
                    </div>

                    <div class="field">
                        <label>Пароль</label>
                        <div class="control">
                            <input type="password" name="password1" class="input" v-model="password1">
                        </div>
                    </div>

                    <div class="field">
                        <label>Повторите пароль</label>
                        <div class="control">
                            <input type="password" name="password2" class="input" v-model="password2">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" :key="error">{{error}}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Отправить</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
  import axios from 'axios';
  import {toast} from 'bulma-toast';
  export default {
      name: 'SignUp',
      data(){
        return {
            email: '',
            password1: '',
            password2: '',
            errors: [],
        }
      },
      methods: {
          submitForm() {
              this.errors = []

              if (this.email === '') {
                  this.errors.push('Введите емайл')
              }

              if (this.password1 === '') {
                   this.errors.push('Введите пароль')
              }

              if (this.password1 !== this.password2) {
                   this.errors.push('Пароль не совпадает')
              }

              if (this.password1.length < 7) {
                   this.errors.push('Слишком короткий пароль')
              }

              if (!this.errors.length) {
                  const formData = {
                      email: this.email,
                      password: this.password1,
                  }

                  axios
                        .post('/api/v1/users/', formData)
                        .then(response => {
                            toast({
                                message: 'Вы зарегистрировались, пожалуйста войдите',
                                type: 'is-success',
                                dismissible: true,
                                pauseOnHover: true,
                                duration: 2000,
                                position: 'bottom-right',
                            })

                            this.$router.push('/log-in')
                        })
                        .catch(error => {
                            if(error.response) {
                                for (const property in error.response.data) {
                                    this.errors.push(`${property}: ${error.response.data[property]}`)
                                }
                            } else if (error.message) {
                                this.errors.push("что-то пошло не так, попробуйте снова!")
                            }      
                        })
              }

              console.log('submitForm')

              console.log(this.email, this.password1, this.password2)
          }
      }
  }
</script>



