<template>
    <h1 v-if="isSuccess == false" style="text-align: center; color: red; margin: 15px 0px 50px 0px;">Сначала авторизируйтесь</h1>
    <div class="main__materials" v-if="Access()">
        <div v-if="isSuccess == true">
            <div v-if="isList == 0">
                <div v-if="isPodActive == 0">
                    <div class="breadcrumb"><router-link class="nav__breadcrumb" to="/">Главная</router-link> / Предметы</div>
                    <hr class="breadcrumb__line">
                </div>
                <div v-if="isPodActive == 0" class="test__inner">
                    <div class="test__menu">
                    <h1>ВЫБОР ПРЕДМЕТА</h1>
                        <div class="link__inner">
                            <div v-for="itemFullMaterials in isFullMaterials" :key="itemFullMaterials">
                                <button :disabled="itemFullMaterials.test_id >= 2" class="test__link" @click="isReadyPodMaterials(itemFullMaterials.test_id), isPodActive = itemFullMaterials.test_id" >
                                    {{ itemFullMaterials.name_materials }}
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="test__menu">
                    <div class="link__inner" v-for="itemPodMaterials in isPodMaterials.length" :key="itemPodMaterials">
                        <div v-if="isPodActive == isPodMaterials[0].test_id">
                            <button :disabled="isPodMaterials[itemPodMaterials - 1].topic_id >= 7" class="test__link" @click="isMaterialActive = isPodMaterials[itemPodMaterials - 1].topic_id, isPodActive = -1">
                                {{isPodMaterials[itemPodMaterials - 1].topic}}
                            </button>
                        </div>
                    </div>   
                </div>

                <div class="materials__full" v-for="itemPodMaterials in isPodMaterials" :key="itemPodMaterials">
                    <div class="materials__inner">
                        <button class="test__ready" v-if="isMaterialActive == itemPodMaterials.topic_id" @click="isList = itemPodMaterials.topic_id, isReadyTest(), isReadyAnswer(itemPodMaterials.topic_id)">Начать тест</button>
                    </div>
                </div>
            </div>

            <div v-for="itemPodMaterials in isPodMaterials" :key="itemPodMaterials" >
                <div class="test__sep" v-if="isList == itemPodMaterials.topic_id">
                    <h1 class="test__name">Тест</h1>
                    <div v-for="itemQuest in isTestQuest.length" :key="itemQuest">
                        <h3 class="quest__test"><div>{{ itemQuest }}: <span>{{ isTestQuestText[itemQuest - 1] }}</span></div> <img :src="isTestQuest[itemQuest - 1]" alt="">  <!--<span>{{ isTestQuest[itemQuest - 1] }}</span>--></h3>
                        <div class="test__quest">
                            <!-- <select v-model="isVerifySelectAnswer[itemQuest - 1]">
                                <option :value="itemAnswer" v-for="itemAnswer in isAnswer[itemQuest - 1]" :key="itemAnswer">
                                    {{ itemAnswer }}
                                </option>
                            </select> -->
                            <input v-model="isVerifySelectAnswer[itemQuest - 1]" type="text" placeholder="Введите ответ">
                        </div>
                    </div>
                    <button @click="isFinishTest" class="test__finish">Завершить тест</button>
                </div>
            </div>

            <div v-if="isList == 'SuccessTest'" class="container">
                <h2>Тест пройден</h2>
                <h3>Баллы: {{isPrecentBalls}}</h3>
                <h4>1 - Правильно</h4>
                <h4>0 - Неправильно</h4>
                <table>
                    <thead>
                        <tr>
                            <th>Число вопросов</th>
                            <th v-for="index in isNextR.length" :key="index">{{ index }}</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Ответы</td>
                            <td v-for="index1 in isNextR.length" :key="index1" :class="isNextR[index1 - 1] === 1 ? 'primary' : 'accent' && isNextR[index1 - 1] === 0 ? 'primaryRed' : 'accent2'">{{isNextR[index1 - 1]}}</td>
                        </tr>
                    </tbody>
                </table>
                 <!-- <Bar style="width: 300px; height: 300px;"
                    :chart-options="chartOptions"
                    :chart-data="chartData"
                    :chart-id="chartId"
                    :dataset-id-key="datasetIdKey"
                    :plugins="plugins"
                    :css-classes="cssClasses"
                    :styles="styles"
                    :width="5"
                    :height="5"
                /> -->
            </div>
            <div v-if="isList == 'FalseTest'">
                <div>
                    <h2 class="h22">Тест не пройден</h2>
                    <table>
                        <thead>
                            <tr>
                                <th>Число вопросов</th>
                                <th v-for="index3 in isNextR.length" :key="index3">{{ index3 }}</th>
                                {{}}
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Ответы</td>
                                <td v-for="index1 in isNextR.length" :key="index1" :class="isNextR[index1 - 1] === 1 ? 'primary' : 'accent' && isNextR[index1 - 1] === 0 ? 'primaryRed' : 'accent2'">{{isNextR[index1 - 1]}}</td>
                            </tr>
                        </tbody>
                    </table>
                    <button class="button2" @click="reloadURL()" to="/materials">Назад</button>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios'
import settings from '../settings'
import Success from './canTest/success.vue'
import Error from './canTest/error.vue'
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
    components: {
        Success,
        Error,
        Bar
    },
    props: {
        chartId: {
        type: String,
        default: 'bar-chart'
        },
        datasetIdKey: {
        type: String,
        default: 'label'
        },
        width: {
        type: Number,
        default: 100
        },
        height: {
        type: Number,
        default: 100
        },
        cssClasses: {
        default: '',
        type: String
        },
        styles: {
        type: Object,
        default: () => {},
        },
        plugins: {
        type: Object,
        default: () => {}
        }
    },

    data() {
        return {
            isMaterialsFalse: "",
            isTestFalse: "",
            isFullMaterials: [],
            isPodMaterials: [],
            isTestQuest: [],
            isTestQuestText: [],
            isAnswer: [],
            isMaterialActive: 0,
            isPodActive: 0,
            isTestActive: 0,
            isSuccess: false,
            isList: "",
            isVerifyAnswer: [],
            isVerifySelectAnswer: [],
            isNextR: [],
            //Materials
            isTraining: false,
            isMethodical: false,
            isPrecentBalls: 0,
            //Active
            isActiveItem: [],

            chartData: {
                labels: [ 'Баллы', 'Проценты' ],
                datasets: [ { 
                    label: 'balls',
                    data: [0,0],
                    backgroundColor: ['lightblue','rgb(255, 99, 132)'],
                } ]
            },
            chartOptions: {
                responsive: true
            }
        }
    },

    mounted: async function () {
        try {
            const resultMaterials = await axios({
                method: 'post',
                url: `${settings.host}/user/materials`,
            })
            this.isFullMaterials = resultMaterials.data.message
        } catch (error) {
            this.isMaterialsFalse = "Материалы не загруженны!"
        }   
    },

    methods: {
        async Access() {
            let token = localStorage.getItem('token')
            if(token == null) {} else {
                try {
                    axios({
                        method: 'post',
                        url: `${settings.host}/user/loginByToken`,
                        headers: {'Authorization': token}
                    })
                    this.isSuccess = true
                } catch (error) {
                    if(error.response.data.message == "jwt expired") {
                        localStorage.removeItem('token')
                    }
                    this.isSuccess = false
                }
            }
        },

        isReadyPodMaterials(isReadyId) {
            try {
                axios.post(`${settings.host}/user/podMaterials`, {
                    test_id: isReadyId
                }).then(response => {
                    this.isPodMaterials = response.data.message
                })
            } catch (error) {
                this.isMaterialsFalse = "Под материалы не загруженны!"
            } 
        },

        isReadyTest() {
            axios.post(`${settings.host}/user/testQuest`, {
                test_id: this.isList
            }).then(response => {
                this.isTestQuest = JSON.parse(response.data.message.quest)
                this.isTestQuestText = JSON.parse(response.data.message.quest_text)
            }).catch(error => {
                this.isTestFalse = "Теста нет!"
            })
        },

        isReadyAnswer(isReadyDo) {
            console.log(isReadyDo)
            axios.post(`${settings.host}/user/testAnswer`, {
                test_id: isReadyDo
            }).then(response => {
                let resultVerifyAnswer = JSON.parse(response.data.message[0].verify_answer)
                let result = JSON.parse(response.data.message[0].answer)
                this.isAnswer = result
                this.isVerifyAnswer = resultVerifyAnswer
            }).catch(error => {
                this.isTestFalse = "Вопросов нет!"
            })
        },

        isFinishTest() {
            let next = 0
            let nextR = []
            for (let indexAnswer = 0; indexAnswer < this.isVerifyAnswer.length; indexAnswer++) {
                if(this.isVerifySelectAnswer[indexAnswer] == this.isVerifyAnswer[indexAnswer]) { //[indexAnswer]
                    next += 1
                    nextR.push(1)
                } else {
                    nextR.push(0)
                }
            }
            console.log(this.isVerifyAnswer)
            let percent = this.isVerifyAnswer.length * 0.5
            if (percent < next) {
                this.chartData.datasets[0]['data'][0] = next 
                this.chartData.datasets[0]['data'][1] = next * 0.5
                this.isPrecentBalls = next
                this.isList = "SuccessTest"
                this.isNextR = nextR

                axios.post(`${settings.host}/user/loading`, {
                    name: localStorage.getItem("name"),
                    number: nextR
                }).then(response => {
                    console.log("Success")
                })

                console.log(nextR)
            } else {
                axios.post(`${settings.host}/user/loading`, {
                    name: localStorage.getItem("name"),
                    number: nextR
                }).then(response => {
                    console.log("False")
                })
                this.isNextR = nextR
                this.isList = "FalseTest"
            }
        },
        reloadURL() {
            location.reload()
        }
    }
}
</script>

<style scoped>
/* table */

table {
    margin: 15px 0px;
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
.h22 {
    text-align: center;
    color: red;
}

.button2 {
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
    text-decoration: none;
}
/* end table */

.main__materials {
    margin: 20px 0px;
}

/* breadcrumb */
.breadcrumb {
    margin: 5px 0px;
}

.nav__breadcrumb {
    color: #000;
    text-decoration: none;
}

.breadcrumb__line {
    margin-bottom: 50px;   
}
/* end breadcrumb */

.test__inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 100px;
}

.test__menu {
  margin: 75px 0px 0px 0px;
}

.test__menu > h1 {
  font-weight: 400;
  font-size: 36px;
  text-align: left;
}

.link__inner {
  display: grid;
  align-items: flex-start;
  flex-wrap: wrap;
  margin: 5px 0px;
}

.link__inner > div > div {
    width: 1000px;
    padding-left: 20px;
}

.test__link {
  border: 2px solid rgb(72, 251, 76);
  padding-left: 10px;
  display: flex;
  align-items: center;
  width: 1000px;
  height: 79px;
  text-decoration: none;
  color: #000;
  margin: 10px;
  font-size: 16px;
  cursor: pointer;
  font-size: 22px;
  box-shadow: rgb(72, 251, 76) 0px 4px 0px;
}

.materials {
    margin: 30px 0px 0px 0px;
    overflow-y: scroll;
    width: 1000px;
    height: 500px;
    background: #eeeeee;
    padding: 10px;
}

.materials__inner {
    display: grid;
    justify-content: center;
}

.materials__pdf {
    margin: 25px 0px;
}

.materials__inner > div > h2 {
    border: 2px solid rgb(72, 251, 76);
    width: 1000px;
    height: 70px;
    display: flex;
    align-items: center;
    padding-left: 10px;
    cursor: pointer;
    margin: 13px 0px;
    box-shadow: rgb(72, 251, 76) 0px 4px 0px;
}

.materials__inner > div > h2 > a {
    text-decoration: none;
    color: #000;
}

.test__ready {
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

.test__finish {
    width: 195px;
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

.test__sep {
    max-width: 900px;
    margin: 0 auto;
}

.test__name {
    margin: 40px 0px;
    text-align: center;
}

.quest__test {
    display: flex;
    align-items: flex-start;
}

.quest__test > div {
    font-weight: 300;
    margin-right: 10px;
}

.test__quest {
    position: relative;
    display: inline-block;
    margin: 22px 0px;
}

/* .test__quest:after {
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
} */

.test__quest > input {
    width: 365px;
    padding-right: 25px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    outline: none;
    height: 53px;
    padding: 0px 10px;
    border: 2px solid rgb(72, 251, 76);
    box-shadow: rgb(72, 251, 76) 0px 4px 0px;
    font-size: 14px;
}

h2 {
    text-align: center;
    color: lightgreen;
}

.verify { 
    display: grid;
    justify-content: center;
    margin: 25px 0px;
}

.verify > a > button {
    width: 80px;
    height: 30px;
    margin: 10px 0px;
}

.verify > a {
    text-decoration: none;
    color: #000;
}

form {
    display: flex;
    flex-direction: column;
    max-width: 350px;
    margin: 50px auto;
}

form > input {
    margin: 5px 0px;
    height: 30px;
    font-size: 20px;
    padding-left: 5px;
}

form > button {
    height: 35px;
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
