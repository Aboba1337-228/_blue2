<template>
    <div v-if="Access()" class="auth">
        <div class="container">
            <div class="auth__logo">
                <h1>Авторизация</h1><img src="../assets/icon/user.png" alt="">
            </div>
            <h1 style="text-align: center; color: lightgreen; margin: 15px 0px;">{{ _success }}</h1>
            <h1 style="text-align: center; color: red; margin: 15px 0px;">{{ _false }}</h1>
            <form class="auth__form" @submit.prevent="submitAuth()">
                <!-- <label>№школы - №класс - №по списку*</label>
                <input v-model="email" type="text" placeholder="56-9А-12"> -->
                <!-- <label>Пароль*</label>
                <input v-model="password" type="password" placeholder="Пароль"> -->
                <label>№Школы*</label>
                <input v-model="NumSchools" type="text" placeholder="">
                <label>№Класс*</label>
                <input v-model="NumClass" type="text" placeholder="">
                <label>№По списку*</label>
                <input v-model="NumSip" type="text" placeholder="">
                <div class="auth__btn">
          <router-link to="/reg" class="further">Зарегистрироваться</router-link>
		          <button class="further">Войти</button>
		 </div>
            </form>
        </div>
    </div>    
</template>

<script>
import axios from 'axios'
import routes from '../router/index'
import settings from '../settings'

export default {
    data() {
        return {
            NumSchools: "",
            NumClass: "",
            NumSip: "",
            email: "",
            password: "",
            _success: "",
            _false: ""
        }
    },

    methods: {
        async submitAuth() {
            try {
                const result = await axios.post(`${settings.host}/user/login`, {
                    numSchools: this.NumSchools,
                    numClass: this.NumClass,
                    numSip: this.NumSip
                })
                localStorage.setItem('token', result.data.token)
                this.redirectProfile()
            } catch (error) {
                this._false = "Логин или Пароль не верный"
            }
        },  

        async Access() {
            let token = localStorage.getItem('token')
            if(token == null) {} else {
                try {
                    axios({
                        method: 'post',
                        url: `${settings.host}/user/loginByToken`,
                        headers: {'Authorization': token}
                    })
                    this.$router.push({path: "/profile"})
                } catch (error) {
                    if(error.response.data.message == "jwt expired") {
                        localStorage.removeItem('token')
                    }
                    this._access = false
                }
            }
        },

        redirectProfile() {
            this.interval = setTimeout(
            function () {
                this.$router.push({path: "/profile"})
            }.bind(this), 1200);
        }
    }
}
</script>

<style scoped>
.auth {
    margin: 120px 0px 0px 0px;
}

.auth__logo {
    text-align: center;
    display: flex;
    justify-content: center;
}

.auth__logo > h1 {
    margin: 0px 25px;
}

.auth__form {
    display: flex;
    flex-direction: column;
    max-width: 365px;
    margin: 0 auto;
}

.auth__form > label {
    margin: 13px 0px 5px 0px;
}

.auth__form > input {
    height: 53px;
    padding-left: 10px;
    font-size: 14px;
    box-shadow: 2px 2px 5px rgba(20, 29, 61, 0.15), -2px -2px 5px rgba(20, 29, 61, 0.15);
    border: 0px;
    color: #939CA6;
    outline: none;
}

.auth__btn {
    display: flex;
    justify-content: right;
    margin: 35px 0px;
}

.reset {
    border: 0px;
    background-color: #fff;
    width: 165px;
    height: 59px;
    font-size: 16px;
    cursor: pointer;
}

.further {
    height: 59px;
    font-weight: 500;
    background: rgb(28, 176, 246);
    border-radius: 7px;
    border: none;
    color: rgb(255, 255, 255);
    box-shadow: rgb(34 155 213) 0px 4px 0px;
    padding: 6px 20px;
    font-size: 20px;
    transition: all 0.3s ease-in-out 0s;
    cursor: pointer;
    box-sizing: border-box;
text-align: center;
display: flex;
margin: 0px 5px;
align-items: center;
text-decoration: none;
}

</style>
