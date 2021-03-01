<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
            type="text"
            name="" id=""
            class="search-bar"
            placeholder="Your City..."
            v-model="url_link"
            @keypress="searchLocation"
        >

        <div class="popup-result" v-show="this.popupResult.status">
          <div class="item">
            <div class="col-left" style="text-align: left">
              <span>Huế</span>
            </div>
            <div class="col-right" style="text-align: right">
              <span>VN</span>
            </div>

          </div>
        </div>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' && weather != 'undefined' ">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},{{ weather.sys.country }}
          </div>
          <div class="date">
            {{ date.day }} - {{ date.month }} - {{ date.year }}
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) / 10 }}°c</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '623949ba087cb249c44a770ad1a293fe',
      url_link: '',
      weather: {},
      date: {day: '', month: '', year: ''},
      popupResult: {
        status: false
      }
    }
  },
  created() {
    this.getDate()
  },
  methods: {
    searchLocation(e) {
      if (e.key == 'Enter') {
        fetch('https://api.openweathermap.org/data/2.5/weather?q=' + this.url_link + '&appid=' + this.api_key + '')
            .then(res => {
              console.log(res)
              if (res.status == 404) {
                console.log(res.status)
              } else
                return res.json();
            }).then(this.setResults)
      }
    },
    setResults(e) {
      this.weather = e;
    },
    getDate() {
      var d = new Date();
      var n = d.getDate();
      var day = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thusday', 'Friday', 'Saturday'];
      var month = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      this.date.day = day[n]
      this.date.month = month[d.getMonth()]
      this.date.year = d.getFullYear()
    },
    showResult() {
      this.popupResult.status = true
      fetch('https://api.openweathermap.org/data/2.5/weather?q=' + this.url_link + '&appid=' + this.api_key + '')
          .then(res => {
            console.log(res)
            if (res.status == 404) {
              console.log(res.status)
            } else
              return res.json();
          }).then(this.setResults)

      setTimeout(function () {
        this.popupResult.status = false
      }, 3000);
    }

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

input {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: 500px auto;
  background-position: center center;
  transition: 0.4s;
  background-repeat: no-repeat;

}

main {
  max-width: 500px;
  margin: 0 auto;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
  position: relative;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: bold;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.popup-result {
  padding: 9px;
  background-color: white;
  position: absolute;
  bottom: -28px;
  width: 100%;
}

.popup-result .item {
  display: flex;
  justify-content: space-between;
}
</style>
