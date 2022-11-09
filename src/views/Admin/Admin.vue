<template>
    <div class="admin">
        <h1>Администрирование</h1>
        <div class="container">
            <div class="admin__inner">
                <div class="admin__sidebar">
                    <h2>Редактирование</h2>
                    <button @click="isListAdmin = 1">Тесты</button>
                    <button @click="isListAdmin = 2">Регионы / Районы</button>
                    <button @click="isListAdmin = 3">Результаты</button>
                </div>
                <div class="admin__main">
                    <div class="main__test" v-if="isListAdmin == 1">
                        <h2>Тесты</h2>
                        <hr>
                        <div class="test__btn">
                            <button @click="isTest = 0">Форма</button>
                            <button @click="isTest = 1">Таблица</button>
                        </div>
                        <hr>
                        <div class="main__test" v-if="isTest == 0">
                            <input type="text" class="test__name" placeholder="Название теста">
                            <input type="text" class="test__name" placeholder="">
                            <button>Загрузить</button>
                        </div>

                        <table v-if="isTest == 1">
                            <thead>
                                <tr>
                                    <th>Номер</th>
                                    <th>Предмет</th>
                                    <th>Вопрос №1</th>
                                    <th>Вопрос №2</th>
                                    <th>Вопрос №3</th>
                                    <th>Вопрос №4</th>
                                    <th>Вопрос №5</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td>1</td>
                                    <td>Физика</td>
                                    <td>1</td>
                                    <td>2</td>
                                    <td>3</td>
                                    <td>4</td>
                                    <td>5</td>
                                </tr>
                                <tr>
                                    <td>2</td>
                                    <td>Математика</td>
                                    <td>1</td>
                                    <td>2</td>
                                    <td>3</td>
                                    <td>4</td>
                                    <td>5</td>
                                </tr>
                                <tr>
                                    <td>3</td>
                                    <td>Химия</td>
                                    <td>1</td>
                                    <td>2</td>
                                    <td>3</td>
                                    <td>4</td>
                                    <td>5</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="main__region" v-if="isListAdmin == 2">
                        <h2>Регион</h2>
                        <hr>
                        <div class="region__btn">
                            <button @click="isRegion = 0">Форма</button>
                            <button @click="isRegion = 1">Таблица</button>
                        </div>
                        <hr>
                        <div class="main__region" v-if="isRegion == 0">
                            <input type="text" class="regoin__name" placeholder="Название региона">
                            <input type="text" class="regoin__name" placeholder="Название района">
                            <button>Загрузить</button>
                        </div>

                        <table v-if="isRegion == 1">
                            <thead>
                                <tr>
                                    <th>Номер</th>
                                    <th>Муниципалитет</th>
                                    <th>Район</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td>1</td>
                                    <td>Оренбург</td>
                                    <td>23 мкр.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="main__result" v-if="isListAdmin == 3">
                        <h2>Результаты теста</h2>
                        <hr>
                        <table>
                            <thead>
                                <tr>
                                    <th>Номер</th>
                                    <th>ФИО участника</th>
                                    <th v-for="index in 15" :key="index">{{ index }}</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="index1 in isResult.length" :key="index1">
                                    <td>{{ isResult[index1 - 1][0] }}</td>
                                    <td>{{ isResult[index1 - 1][1] }}</td>
                                    <td v-for="index2 in isResult[index1 - 1][2].length" :key="index2" :class="isResult[index1 - 1][2][index2 - 1] === 1 ? 'primary' : 'accent' && isResult[index1 - 1][2][index2 - 1] === 0 ? 'primaryRed' : 'accent2'">{{ isResult[index1 - 1][2][index2 - 1] }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios'
import settings from '../../settings'

export default {
    data() {
        return {
            isListAdmin: 1,
            isRegion: 0,
            isTest: 0,
            isResult: []
        }
    },
    mounted: async function() {
            try {
                const resultTest = await axios({
                    method: 'post',
                    url: `${settings.host}/user/unloading`,
                })
                for (let index = 0; index < resultTest.data.message.length; index++) {
                    this.isResult.push([resultTest.data.message[index].id, resultTest.data.message[index].FIO, JSON.parse(resultTest.data.message[index].number)])                   
                }
            } catch (error) {
                console.log("Материалы не загруженны!")
            }   
        }
}
</script>

<style scoped>
h1 {
    text-align: center;
    margin: 20px 0px;
}

th, td {
    border: 1px solid #000;
}

td {
    padding: 0px 5px;
}

.primary {
    background: lightgreen;
}

.primaryRed {
    background: lightcoral;
}

.admin__inner {
    display: flex;
}

.admin__sidebar {
    border: 1px solid #000;
    flex-direction: column;
    display: flex;
    width: 200px;
    height: 484px;
    padding: 10px;
}

.admin__sidebar > h2 {
    text-align: center;
    margin-bottom: 10px;
}

.admin__sidebar > button {
    padding: 5px 0px;
    margin: 5px 0px;
    cursor: pointer;
}

.admin__main {
    margin: 0px 0px 0px 50px;
}

/* test */
.main__test {
    display: flex;
    flex-direction: column;
    width: 700px;
}

.main__test > hr {
    margin: 20px 0px;
}

.test__btn > button {
    padding: 5px 20px;
    margin-right: 10px;
    cursor: pointer;
}

.main__test > input, textarea {
    margin: 10px 0px;
    font-size: 20px;
}

.main__test > button {
    padding: 10px 0px;
    width: 200px;
    cursor: pointer;
}

.test__name, textarea {
    padding: 5px 0px;
    padding-left: 5px;
}
/* end test */

/* regoin */
.main__region {
    display: flex;
    flex-direction: column;
    width: 700px;
}

.main__region > hr {
    margin: 20px 0px;
}

.region__btn > button {
    padding: 5px 20px;
    margin-right: 10px;
    cursor: pointer;
}

.main__region > input, textarea {
    margin: 10px 0px;
    font-size: 20px;
}

.main__region > button {
    padding: 10px 0px;
    width: 200px;
    cursor: pointer;
}

.regoin__name, textarea {
    padding: 5px 0px;
    padding-left: 5px;
}
/* end region */

/* result */
.main__result {
    display: flex;
    flex-direction: column;
    width: 1000px;
}

.main__result > hr {
    margin: 20px 0px;
}
/* end result */
</style>
