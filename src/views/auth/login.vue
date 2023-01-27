<template>
  <div class="container">
    <form class="login" @submit.prevent="store()">
      <h1>Login to your Account</h1>
      <label for="">Email Address</label>
      <input type="email" placeholder="email@example.org" v-model="login.email" required>
      <div v-if="validation.email">
        {{ validation.email[0] }}
      </div> 
      <label for="">Password</label>
      <input type="password" v-model="login.password" id="psw" name="psw" placeholder="********" pattern="(?=.).{8,}"
        title="Harus berisi setidaknya 8 karakter atau lebih" required>
      <div v-if="validation.password">
        {{ validation.password[0] }}
      </div>
      <button>Login</button>
    </form>
    <h5>Don't have an account?<router-link :to="{ name: 'auth.register' }" class="router">&nbsp
        Register</router-link>
    </h5>
    <h5>Forgotten your password? <a class="router">&nbsp Recover Password</a></h5>
  </div>
</template>

<script>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
  setup() {
    const login = reactive({
      email: '',
      password: '',
    });

    const validation = ref([]);
    const router = useRouter();

    function store() {
      axios.post(
        'http://127.0.0.1:8000/api/login',
        login
      )
        .then(() => {
          router.push({
            name: 'login.index'
          });
        }).catch((err) => {
          validation.value = err.response.data
        });
    }
    console.log(store)
    return {
      login,
      validation,
      router,
      store

    }


  }
}
</script>
