<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 24 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Buscar ciudad..."
          v-model="query"
          @keypress="fetchWeather"

         
          
        />
      </div>

      <div class="weather-wrap" v-if='typeof weather.main != "undefined"' >
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
             <div class="weather"> {{ weather.weather[0].main }} </div>

          <div class="container">
                <div style="float:left; margin-left:15%;">
              <p class="weatherTxt" >Temperatura Actual:</p>

                <div class="temp" >{{ Math.round(weather.main.temp) }}°c</div>

                    </div>
                  
                    <div style="float:left; margin-left:5%; ">
                      <p class="weatherTxt" >Temperatura Max:</p>
                  <div class="temp" >{{  Math.round(weather.main.temp_max) }}°c</div>

                    </div>
                    <div style="float:left; margin-left:5%; ">
                      <p class="weatherTxt">Temperatura min:</p>

                      <div class="temp" >{{ Math.round(weather.main.temp_min) }}°c</div>

          </div>

      <div>
  <b-button v-b-toggle.collapse-2 class="m-1" variant="primary">Mas Informacion</b-button>


  <b-collapse id="collapse-2">
    <b-card >
      <div>
        <p style="color:red;">Velocidad del viento:</p>{{ weather.wind.speed }} mts/seg
      </div>
      <div>
        <p style="color:red;">Humedad:</p>{{ weather.main.humidity }} %
      </div>
      <div>
        <p style="color:red;">Presion:</p>{{ weather.main.pressure }} hPa
      </div>
      <div>
        <p style="color:red;">Hora amanecer:</p>{{ this.formatoAmanecer }} 
      </div>
      <div>
        <p style="color:red;">Hora atardecer:</p>{{ this.formatoAtardecer }}
      </div>
      
      



    </b-card>
  </b-collapse>
</div>
          



          </div>
           
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '5299e83c9cf2fef510fb8ddaa3206ac8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
 

  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
      console.log(this.weather);
      this.weather.amanecer=this.weather.sys.sunrise;
      this.weather.atardecer=this.weather.sys.sunset;
      this.horaAmanecer=new Date(this.weather.amanecer*1000);
      this.horaA=this.horaAmanecer.getHours();
      this.minuA="0" + this.horaAmanecer.getMinutes();
      this.segA= "0" + this.horaAmanecer.getSeconds();
      this.formatoAmanecer = this.horaA + ':' + this.minuA.substr(-2) + ':' + this.segA.substr(-2);

      this.horaAtardecer=new Date(this.weather.atardecer*1000);
      this.horaT=this.horaAtardecer.getHours();
      this.minuT="0" + this.horaAtardecer.getMinutes();
      this.segT= "0" + this.horaAtardecer.getSeconds();
      this.formatoAtardecer = this.horaT + ':' + this.minuT.substr(-2) + ':' + this.segT.substr(-2);


    },
    dateBuilder () {
      let d = new Date();
      let months = ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto", "Septiembre", "Octumbre", "Noviembre", "Diciembre"];
      let days = ["Domingo", "Lunes", "Martes", "Miercoles", "Jueves", "Viernes", "Sabado"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    
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

#app {
 background-image: url('./assets/lluvia.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/playa.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
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
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weatherTxt {
  color: #FFF;
  font-size: 30px;
  font-weight: 500;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
