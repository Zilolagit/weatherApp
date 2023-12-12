<template>
  <div>
    <div class="weather">
      <div :class="{ weather__card: degree && list, weather__cold: !degree && list }" class="weather__bg">
        <form @submit.prevent="search">
          <input type="text" v-model="word" class="weather__input" placeholder="Shahar nomini kiriting">
        </form>
        <div class="weather__infos" v-if="loaded">
          <h3 class="degree"> <span>{{ list.name }},</span> <span>{{ list.sys.country }}</span></h3>
          <p>{{ date }}</p>
          <div class="weather__cg ">
            <div class="weather__pict">
              <template v-for="weather in list.weather">
                <div class="weather__weat" v-if="weather.main == 'Clouds'">
                  <img class="cloudy" src="../src/assets/cloud.svg" alt="cloudy">
                </div>
                <div class="weather__weat" v-if="weather.main == 'Rain'">
                  <img class="cloudy" src="../src/assets/rainy.svg" alt="rainy">
                </div>
                <div class="weather__weat" v-if="weather.main == 'Clear'">
                  <img class="cloudy" src="../src/assets/sun.svg" alt="clear">
                </div>
                <div class="weather__weat" v-if="weather.main == 'Haze'">
                  <img class="cloudy" src="../src/assets/clound&sun.svg" alt="clear">
                </div>
                <div class="weather__weat" v-if="weather.main == 'Smoke'">
                  <img class="cloudy" src="../src/assets/fog.svg" alt="clear">
                </div>

              </template>
              <div class="weather__box">
                <p>{{ Math.round(list.main.temp) }} &deg;C</p>
              </div>
            </div>
          </div>

          <template v-for="weather in list.weather">
            <p class="weather__subt">{{ weather.main }}</p>

          </template>



        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
  data() {
    return {
      date: "",
      word: "",
      list: null,
      loaded: false,
      degree: false
    }
  },
  async mounted() {
    let now = new Date();
    this.date = String(now).slice(0, 15)
  },
  methods: {
    async search() {
      try {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.word}&units=metric&APPID=38c126d5e32eab21fd35a4d2f1fb0881`)
        this.list = response.data
        this.loaded = true
        if (Math.round(this.list.main.temp) >= 16) {
          this.degree = true
        } else {
          this.degree = false
        }
        this.word = ""
      } catch (error) {
        if (error.response) {
          if (error.response.status == 404) {
            toast.error(`${error.response.data.message}`, {
              autoClose: 1000,
              theme: "colored"
            });
            console.log(error.response);
            this.word = ""
          }
        }
      }

    }
  }

}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

img {
  max-width: 100%;
  width: 80px;
}

body {
  margin: 0;
  padding: 0;
}

.weather {
  padding-top: 50px;
  padding-bottom: 60px;
  background-position: bottom center;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #000;
  position: relative;
  padding: 0 25px;
  text-align: center;
  &__subt {
    font-size: 35px !important;
    font-style: italic;
    color: #fff !important;
    font-weight: 700;
    font-family: Arial, Helvetica, sans-serif;
  }
  &__cg {
    padding-bottom: 30px;
  }


  &__pict {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__bg {
    transition: background-image .6s ease-in-out;
  }

  &__card {
    height: 100%;
    width: 100%;
    height: 480px;
    width: 360px;
    transition: all .3s ease;
    border-radius: 5px;
    background-image: url(../src/assets/warm-bg.jpg);
    box-shadow: 0px 0px 10px 0px #fff;
    transition: background-image .3s ease;
    padding: 25px;
  }

  &__cold {
    height: 100%;
    width: 100%;
    height: 480px;
    width: 360px;
    transition: all .3s ease;
    border-radius: 5px;
    background-image: url(../src/assets/cold-bg.jpg);
    box-shadow: 0px 0px 10px 0px #fff;
    transition: background-image .3s ease;
    padding: 25px;

  }

  &__input {
    width: 100%;
    border-top-right-radius: 15px;
    border-bottom-left-radius: 15px;
    border: none;
    padding: 12px 20px;
    background-color: rgb(248, 239, 239);
    border: 1px solid #aaa;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    font-weight: 700;
    font-size: 18px;
  }

  &__input:focus-visible {
    outline: none;
  }

  &__infos {
    >h3 {
      color: #fff;
      font-size: 40px;
      margin-top: 25px;
    }

    >p {
      color: #000;
      font-weight: 300;
      font-size: 18px;
      margin-top: 12px;
      margin-bottom: 30px;

    }
  }

  &__box {

    width: 50%;
    background-color: #ffffffb2;
    padding: 30px 15px;
    font-size: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 5px 4px 0px 3px #5f5f5f51;
    margin: 0 auto;
    border-radius: 10px;
    margin-bottom: 30px;
    margin: 0;
    margin: 0 auto;

    * {
      font-weight: 900;
      font-size: 38px;
      font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }
  }



}
</style>