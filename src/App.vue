<template>
  <header v-if="nameUser() && $route.meta.header === 1" class="header">
    <div class="container">
      <div class="header__inner">
        <div class="header__logo">
          <router-link class="logo" to="/"> <h2 style="color: #000;">ОРЕНБУРГСКАЯ <br> ЭЛЕКТРОННАЯ ШКОЛА</h2> </router-link>
        </div>
        <router-link v-if="_success == false" class="header__auth" to="/auth">
          <span class="span">Войти</span>
        </router-link>

        <router-link v-if="_success == true" class="header__auth" to="/profile">
          <span class="span">{{ username }}</span>
        </router-link>
      </div>
    </div>
  </header>
  <router-view class="view"/>
  <footer v-if="$route.meta.header === 1" class="footer">
    <div class="container">
      <div class="footer__inner">
        <div>ул. Чкалова, 11, Оренбург,<br> Оренбургская обл., 460001</div>
        <div>
          <a href="#"><img width="23" height="19" src="./assets/footer/vk.png"></a>
          <a href="#"><img width="19" height="19" src="./assets/footer/telegram.png"></a>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
import axios from 'axios'
import settings from './settings'

export default {
  data() {
    return {
      _success: false,
      username: ""
    }
  },
  methods: {
    async nameUser() {
      let token = localStorage.getItem('token')
      if(token == null) {} else {
          try {
              await axios({
                  method: 'post',
                  url: `${settings.host}/user/loginByToken`,
                  headers: {'Authorization': token}
              })
              const infoUser = await axios({
                  method: 'get',
                  url: `${settings.host}/user/infoByToken`,
                  headers: {'Authorization': token}
              })
              console.log(infoUser.data.user.name)
              localStorage.setItem("name", infoUser.data.user.number)
              this.username = infoUser.data.user.number
              this._success = true
          } catch (error) {
              if(error.response.data.message == "jwt expired") {
                  localStorage.removeItem('token')
              }
              this._success = false
          }
      }
    }
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  font-family: "SFProDisplay";
}

.container {
  max-width: 1280px;
  margin: 0 auto;
}

@font-face {
  font-family: "SFProDisplay";
  src: url(fonts/SFProDisplay-Regular.ttf) format("truetype");
}

.view {
  min-height: calc(100vh - 70px);
}
</style>

<style scoped>
.header {
  margin: 10px 0px;
}

.header__inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__logo {
  text-align: left;
}

.logo {
  text-decoration: none;
}

.nav__link {
  font-weight: 400;
  font-size: 18px;
  text-decoration: none;
  color: #000;
  margin: 0px 25px;
}

.header__phone {
  font-weight: 300;
}

.header__auth {
  display: flex;
  align-items: center;
  text-decoration: none;
}

.header__auth > span {
  font-size: 18px;
  margin: 0px 13px 0px 0px;
  color: #fff;
}

.span {
  font-size: 18px;
  margin: 0px 13px 0px 0px;
  color: #fff;
  text-decoration: none;
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

.footer {
  background-color: rgb(28, 176, 246);
  padding: 15px 0px;
  align-items: center;
  margin: 55px 0px 0px 0px;
}

.footer__inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.footer__inner > div {
  color: #fff;
}

.footer__inner > div > a {
  margin: 0px 7px;
}

</style>
