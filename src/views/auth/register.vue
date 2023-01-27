<template>
    <div>
        <form class="register" @submit.prevent="store()">
            <h1>Create Your Account</h1>
            <label for="">First Name</label>
            <input type="text" placeholder="Joko" v-model="register.firstname" required>
            <div v-if="validation.firstname">
                {{ validation.firstname[0] }}
            </div>
            <label for="">Last Name</label>
            <input type="text" placeholder="Shidiq" v-model="register.lastname" required>
            <div v-if="validation.lastname">
                {{ validation.lastname[0] }}
            </div>
            <label for="">Email</label>
            <input type="email" placeholder="email@example.org" v-model="register.email" required>
            <div v-if="validation.email">
                {{ validation.email[0] }}
            </div>
            <label for="">Password</label>
            <input type="password" v-model="register.password" id="psw" name="psw" placeholder="********"
                pattern="(?=.).{8,}" title="Harus berisi setidaknya 8 karakter atau lebih" required>
            <div v-if="validation.password">
                {{ validation.password[0] }}
            </div>
            <div class="tandc">
                <input type="checkbox" class="in">
                <label for="">To register with us please tick to agree to our <a class="terms">Terms and
                        Conditions</a>.</label>
            </div>
            <button onclick="myalert() ">Register</button>

        </form>
        <h5>Already have an account?<router-link :to="{ name: 'auth.login' }" class="router">&nbsp Sign in</router-link>
        </h5>
    </div>
    <div id="message">
        <h3> Password harus terdiri dari: </h3>
        <p id="length" class="invalid"> Minimal <b> 8 karakter </b> </p>
    </div>
</template>

<script>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
    setup() {
        const register = reactive({
            firstname: '',
            lastname: '',
            email: '',
            password: '',
        });

        const validation = ref([]);
        const router = useRouter();

        function store() {
            axios.post(
                'http://127.0.0.1:8000/api/register',
                register
            )
                .then(() => {
                    router.push({
                        name: 'dashboard'
                    });
                }).catch((err) => {
                    validation.value = err.response.data
                });
        }

        return {
            register,
            validation,
            router,
            store
        }
    }
}
</script>
