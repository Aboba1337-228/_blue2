<template>
    <div v-if="Access()" class="reg">
        <div class="container">
            <div class="reg__logo">
                <h1>Регистрация</h1><img src="../assets/icon/user.png" alt="">
            </div>
            <h1 style="text-align: center; color: lightgreen; margin: 15px 0px;">{{ _success }}</h1>
            <h1 style="text-align: center; color: red; margin: 15px 0px;">{{ _false }}</h1>
            <form class="reg__form" @submit.prevent="submitPost()">
                <!-- <label>ФИО*</label>
                <input v-model="fio" type="text" placeholder="ФИО">
                <label>Номер телефона*</label>
                <input v-model="phone" type="text" placeholder="Телефон"> -->
                <label>№Школы*</label>
                <input v-model="NumSchools" type="text" placeholder="">
                <label>№Класс*</label>
                <input v-model="NumClass" type="text" placeholder="">
                <label>№По списку*</label>
                <input v-model="NumSip" type="text" placeholder="">
                <!-- <label>Пароль*</label>
                <input v-model="password" type="password" placeholder="Пароль"> -->
                <label>Муниципалитет*</label>
                <div class="reg__region">
                    <select v-model="region" name="region">
                        <option value="">Выберите муниципалитет</option>
                        <option :value="isMynicipal.mynicipal" v-for="isMynicipal in mynicipal" :key="isMynicipal">{{isMynicipal.mynicipal}}</option>
                    </select>
                </div>
                <label>Образовательное учереждение*</label>
                <div class="reg__schools">
                    <!-- <select v-model="isSchools" name="region">
                        <option value="">Выберите образовательное учереждение</option>
                        <option :value="isFullSchools.schools" v-for="isFullSchools in schools" :key="isFullSchools">{{isFullSchools.schools}}</option>
                    </select> -->
                    <input type="text" v-model="isSchools" placeholder="Название образовательного учереждения">
                </div>
                <div class="reg__btn">
                    <button class="further">Далее</button>
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
            fio: "",
            phone: "",
            email: "",
            password: "",
            region: "",
            isSchools: "",
            _success: "",
            _false: "",
            mynicipal: [],
            schools: [],
            interval: null
        }
    },

    mounted: async function() {
        try {
            axios.post(`${settings.host}/user/mynicipal/`).then(response => {
                this.mynicipal = response.data.message
            })
            axios.post(`${settings.host}/user/schools/`).then(response => {
                this.schools = response.data.message
            })
        } catch (error) {
            console.log(error)
        }
    },

    methods: {
        submitPost() {
            axios.post(`${settings.host}/user/register/`, {                
                numSchools: this.NumSchools,
                numClass: this.NumClass,
                numSip: this.NumSip,
                city: this.region,
                schools: this.isSchools
            }).then(response => {
                console.log(response.data)
                this._success = "Вы усшено зарегистрированны"
                this._false = ""
             
                this.redirectAuth()
            }).catch(e => {
                console.log(e)
                this._false = "Ошибка регистрации"
                this._success = ""
            })
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
        redirectAuth() {
            this.interval = setTimeout(
            function () {
                this.$router.push({path: "/auth"})
            }.bind(this), 1200);
        }
    }
}
</script>

<style scoped>
.reg {
    margin: 20px 0px 0px 0px;
}

.reg__logo {
    text-align: center;
    display: flex;
    justify-content: center;
}

.reg__logo > h1 {
    margin: 0px 25px;
}

.reg__form {
    display: flex;
    flex-direction: column;
    max-width: 365px;
    margin: 0 auto;
}

.reg__form > label {
    margin: 13px 0px 5px 0px;
}

.reg__form > input {
    height: 53px;
    padding-left: 10px;
    font-size: 14px;
    box-shadow: 2px 2px 5px rgba(20, 29, 61, 0.15), -2px -2px 5px rgba(20, 29, 61, 0.15);
    border: 0px;
    color: #939CA6;
    outline: none;
}

.reg__region {
    position: relative;
    display: inline-block;
}

.reg__region:after {
    content: "▼";
    padding: 0 8px;
    font-size: 12px;
    position: absolute;
    right: 8px;
    top: 20px;
    z-index: 1;
    text-align: center;
    width: 10%;
    height: 100%;
    pointer-events: none;
    box-sizing: border-box;
}

.reg__region > select {
    width: 365px;
    padding-right: 25px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    outline: none;
    height: 53px;
    padding-left: 10px;
    font-size: 14px;
    box-shadow: 2px 2px 5px rgba(20, 29, 61, 0.15), -2px -2px 5px rgba(20, 29, 61, 0.15);
    border: 0px;
    color: #939CA6;
    outline: none;
}

.reg__schools > input {
    width: 330px;
    padding-right: 25px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    outline: none;
    height: 53px;
    padding-left: 10px;
    font-size: 14px;
    box-shadow: 2px 2px 5px rgba(20, 29, 61, 0.15), -2px -2px 5px rgba(20, 29, 61, 0.15);
    border: 0px;
    color: #939CA6;
    outline: none;
}

.reg__btn {
    display: flex;
    justify-content: space-between;
    margin: 35px 0px;
}

.further {
    width: 165px;
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
}

</style>
