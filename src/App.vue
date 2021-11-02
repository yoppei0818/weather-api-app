<template>
  <div id="app" class="">
    <div class="search-box">
      <input 
        type="text" 
        class="search-text" 
        placeholder="例） Tokyo"
        v-model = "country"
      >
      <button @click="searchWeather" class="search-btn">天気を見る</button>
    </div>

    <div class="weather-container" v-if="result">
      <div class="location-box">
        <p class="location">{{ weather.name }}、{{ weather.sys.country }}</p>
        <p class="date">{{ time }}</p>
      </div>

        <div class="weather-box">
          <div class="temp">
            <p>{{ Math.floor(weather.main.temp) }}℃</p>
          </div>
          <p class="weather">{{ setWeather(weather.weather[0].main) }}</p>
        </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: function() {
    return {
      api_key: '4a8b789a5a01911f2b5624b648028a4a',
      url_base: 'https://api.openweathermap.org/data/2.5/weather?q=',
      country: '',
      weather: {},
      time: '',
      result: false,

    }
  },
  methods: {
    searchWeather: function() {
      let url = this.url_base + this.country + "&units=metric&lang=ja&APPID=" + this.api_key
      this.axios.get(url)
        .then((res) => {
          console.log(res.data);
          this.weather = res.data
          this.setDate()
          this.result = true
        })
        .catch((error) => {
          console.log(error);
        });
    },
    setDate: function() {
      let now = new Date()
      this.time = now.getFullYear() + '年' + now.getMonth() + '月' + now.getDate() + '日'
    },
    setWeather:function(weather) {
      switch(weather) {
        case "Suny":
          weather = "晴れ";
          break;
          
        case "Clouds":
          weather = "曇り";
          break;
          
        case "Rain":
          weather = "雨";
          break;
      }
      return weather;
    },
  }
}
</script>

<style lang="scss">
@import "./css/reset";

::placeholder {
  color: rgba(0, 0, 0, 0.808);
}

#app {
  background-image: url("../public/images/山の風景.jpg");
  background-size: cover;
  min-height: 100vh;

  .search-box {
    padding-top: 30px;

    .search-text {
      display: block;
      margin: 0 auto;
      padding: 20px 15px;
      width: 50%;
      border-radius: 30px;
      color: #000;
      border: 1px solid rgb(177, 177, 177);
      background-color: rgba(255, 255, 255, 0.9);
      font-size: 14px;

      &:focus {
        background-color: rgba(255, 255, 255, 1);
      }
    }

    .search-btn {
      width: 150px;
      display: block;
      margin: 0 auto;
      margin-top: 20px;
      padding: 10px;
      border-radius: 30px;
      cursor: pointer;
      color: rgb(21, 158, 73);
      font-weight: 600;
      border: 3px solid rgb(21, 158, 73);
      background-color: #fff;
      transition: 0.3s;

      &:hover {
        color: #fff;
        border: 3px solid rgb(21, 158, 73);
        background-color: rgb(21, 158, 73);
      }
    }
  }

  .weather-container {
    width: 600px;
    margin: 0 auto;
    margin-top: 80px;

    .location-box {
      width: 500px;
      margin: 0 auto;
      padding: 10px;
      text-align: center;

      .location {
        color: rgba(0, 0, 0, 0.9);
        font-weight: 600;
        font-size: 38px;
        margin-bottom: 10px;
        text-shadow: 0 2px 5px rgba(170, 170, 170, 0.9);
      }

      .date {
        color: rgba(0, 0, 0, 0.9);
        font-weight: 600;
        font-size: 16px;
        letter-spacing: 1px;
        text-shadow: 0 2px 5px rgba(170, 170, 170, 0.9);
      }
    }

    .weather-box {
      width: 500px;
      margin: 0 auto;
      padding: 10px;
      text-align: center;
      margin-top: 20px;

      .temp {
        margin: 0 auto;
        width: 280px;
        background-color: rgba(255, 255, 255, 0.8);
        box-shadow: 2px 2px 10px #000;
        padding: 50px 0px;
        border-radius: 10px;


        p {
          color: rgba(0, 0, 0, 0.9);
          font-size: 45px;
          font-weight: 800;
        }
      }

      .weather {
        margin-top: 50px;
        font-size: 40px;
        font-weight: 800;
        color: #fff;
        text-shadow: 0 2px 10px rgba(0, 0, 0, 0.9);
      }
    }
  }
}
</style>
