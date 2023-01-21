<script>
import axios from 'axios'

export default {
    data() {
        return {
            city: "",
            error: "",
            info: null
        }
    },
    computed: {
        cityName() {
            return "«" + this.city + "»"
        },
        showTemp() {
            return "Температура: " + this.info.temp
        },
        showFeelsLike() {
            return "Ощущается как: " + this.info.feels_like
        },
        showMinTemp() {
            return "Минимальная температура: " + this.info.temp_min
        },
        showMaxTemp() {
            return "Максимальная температура: " + this.info.temp_max
        }
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2){
                this.error = "Нужно название больше одного символа"
                return false;
            }
            this.error = ""

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=586c092f9d86c7b5413a3412bb19e319&units=metric`)
                .then(res => (this.info = res.data.main))
        }
    }
}
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == "" ? "Вашем городе" : cityName}}</p>
        <input type="text" v-model="city" placeholder="Введите город">
        <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
        <button disabled v-else="city != ''">Введите название города</button>
        <p class="error">{{ error }}</p>

        <div v-if="info !== null">
            <p>{{ showTemp }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>

<style scoped>
.wrapper{
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #3a849b;
    text-align: center;
    color: #fff;
}

.wrapper h1 {
    margin-top: 50px;
}
.wrapper p {
    margin-top: 20px;
}

.wrapper input{
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #fff;
    color: #fff;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}
.wrapper input::placeholder{
    color: #fff;
}
.wrapper input:focus{
    border-bottom-color: rgb(165, 155, 155);
}

.wrapper button {
    background: #17a7d3;
    color: #fff;
    border-radius: 10px;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
}
.wrapper button:hover{
    transform: scale(1.1) translateY(-5px);
}

.wrapper button:disabled{
    background: #08607a;
    cursor: not-allowed;
}
.error {
    color: red;
}
</style>
