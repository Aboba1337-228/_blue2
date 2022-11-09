<template>
  <div class="home">
    <div class="container">
      <div class="home__inner">
        <div class="home__menu">
          <h1>ДОБРО ПОЖАЛОВАТЬ</h1>
          <div class="link__inner">
            <router-link class="home__link" to="/materials">Обучение</router-link>
            <router-link class="home__link" to="/materials">Тестирование</router-link>
          </div>
        </div>
        <img src="../assets/home/main.png" alt="">
      </div>
      <div class="number__one">
        <img src="../assets/home/relationship.png" alt="">
        <div>
          <h2>Ресурс №1 для изучения биологии, химии и физики</h2>
          <p>Учиться с ОЭШ весело и интересно. Зарабатыайте очки за правильные ответы, открывайте новые материалы и узнавайте новое. Наши видео-уроки помогут вам в этом</p>
        </div>
      </div>
      <div class="cluster">
        <h2>Почему вам обязательно понравится ОЭШ?</h2>
        <div class="cluster__inner">
          <div>
            <div>
              <img width="60" height="60" src="../assets/home/fire.png" alt="">
              <div>
                <h2>Быстрый прогресс</h2>
                <p>Исследования показали, что наши курсы эффективно развивают навыки чтения, устной речи и понимания на слух</p>
              </div>
            </div>
            <div>
              <img width="60" height="60" src="../assets/home/girl.png" alt="">
              <div>
                <h2>Индивидуальное обучение</h2>
                <p>Наши уроки сочетают лучшие достижения искусственного интеллекта и лингвистики. Мы балансируем сложность и темп обучения для каждого пользователя индивидуально!</p>
              </div>
            </div>
          </div>

          <div>
            <div>
              <img width="60" height="60" src="../assets/home/brain.png" alt="">
              <div>
                <h2>Стимул к учебе</h2>
                <p>С нашим игровым подходом ежедневные занятия входят в привычку. Вам помогут забавные задания и напоминания от нашего талисмана — совёнка Duo</p>
              </div>
            </div>
            <div>
              <img width="60" height="60" src="../assets/home/pen.png" alt="">
              <div>
                <h2>Нескучные уроки</h2>
                <p>Эффективное обучение не должно быть скучным! Интересные задания и забавные персонажи дают стимул заниматься каждый день</p>
              </div>
            </div>
          </div>

        </div>
      </div>

      <div class="number__one">
        <img src="../assets/home/Technologies.png" alt="">
        <div>
          <h2>Ресурс №1 для изучения биологии, химии и физики</h2>
          <p>Учиться с ОЭШ весело и интересно. Зарабатыайте очки за правильные ответы, открывайте новые материалы и узнавайте новое. Наши видео-уроки помогут вам в этом</p>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
.home {
  margin: 100px 0px 0px 0px;
}

.home__inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 100px;
}

.home__menu > h1 {
  font-weight: 400;
  font-size: 36px;
}

.link__inner {
  display: grid;
  margin: 55px 0px;
}

.home__link {
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 395px;
  height: 59px;
  text-decoration: none;
  color: #000;
  margin: 10px 0px;
  font-size: 16px;
}

.number__one {
  display: flex;
  align-items: center;
  margin: 80px 0px;
}

.number__one > div {
  margin-left: 30px;
}

.number__one > div > h2 {
  margin: 10px 0px;
  font-size: 35px;
  width: 690px;
}

.number__one > div > p {
  width: 590px;
}

.cluster {
  margin: 45px 0px;
}

.cluster__inner {
  display: flex;
  justify-content: space-between;
}

.cluster__inner > div > div {
  display: flex;
  align-items: center;
  margin: 35px 0px;
}

.cluster__inner > div > div > img {
  margin-right: 15px;
}

.cluster__inner > div > div > div > h2 {
  margin: 10px 0px;
}

</style>
