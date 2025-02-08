<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "<<" + this.city + ">>";
    },
    showTemp() {
      return "Температура:" + this.info.main.temp;
    },
    showFeelsLike() {
      return "Відчувається як:" + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Мінімальна температура:" + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Максимальна температура:" + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Потрібна назва більше одного символу:)";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=d196832d2a8ebb81906f22a2c71f8b44`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Програма погоди</h1>
    <p>Дізнатися погоду в {{ city == "" ? "вашому місті" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введіть місто" />
    <button v-if="city != ''" @click="getWeather()">Дізнатися погоду</button>
    <button disabled v-else>Введіть назву міста</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #ec5c08;
}
.wrapper {
  width: 600px;
  height: 400px;
  border-radius: 20px;
  padding: 20px;
  background: #170481;
  text-align: center;
  color: aliceblue;
}
.wrapper h1 {
  margin-top: 30px;
}
.wrapper p {
  margin-top: 30px;
}
.wrapper input {
  margin-top: 20px;
  background: transparent;
  border: 0;
  border-bottom: 1px solid #d9d5e7;
  color: rgb(11, 63, 109);
  font-size: 20px;
  padding: 10px 15px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: aqua;
}

.wrapper button:disabled {
  background: #ec5c08;
  cursor: not-allowed;
}
.wrapper button {
  background: #77ec08;
  color: #170481;
  border-radius: 10px;
  border: 2px solid #d9d5e7;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
