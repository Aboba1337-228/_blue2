<template>
    <div>
        <h2>Тест не пройден</h2>
        <button @click="reloadURL()" to="/materials">Назад</button>
    </div>
</template>

<style scoped>
h2 {
    text-align: center;
    color: red;
}

button {
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
</style>

<script>
export default {
    mounted() {
        window.scrollTo({ top: 0 });
    },
    methods: {
        reloadURL() {
            location.reload()
        }
    }
}
</script>