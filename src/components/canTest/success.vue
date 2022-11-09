<template>
    <div class="container">
        <h2>Тест пройден</h2>
        <h3>Баллы: {{isPrecentBalls}}</h3>
    </div>
</template>

<style scoped>
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

<script>
export default {

    data() {
        return {
            isPrecentBalls: 0
        }
    },

    mounted() {
        window.scrollTo({ top: 0 });
    }
}
</script>