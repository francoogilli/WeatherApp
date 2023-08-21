<template>
  <div class="home">
    <div class="wrapper">
            <div class="sidebar">
              <div>
                <div class="search">
                  <input type="text" v-model="searchQuery" placeholder="Enter a place name">
                  <select @change="handleSelectionChange" >
                      <option  v-for="place in searchResults"
                              :value="place.display_name"
                              :key="place.place_id">
                          {{ place.display_name }}
                      </option>
                  </select>
              </div>

                <div class="weather-icon">
                  <img src="../assets/img/weather/03d.png" class="day-icon" alt="" />
                </div>
                <div class="temperature">
                    <h1 id="temp">28</h1>
                  <span class="temp-unit">°C</span>
                </div>
                <div class="date-time">
                  <p id="date-time">Mendoza,AR</p>
                </div>
                <div class="divider"></div>
                <div class="condition-rain">
                  <div class="condition">
                    <i class="fas fa-cloud"></i>
                    <p id="condition">Rain</p>
                  </div>
                  <div class="rain">
                    <i class="fas fa-tint"></i>
                    <p id="rain">Rain ligh with drops</p>
                  </div>
                </div>
              </div>
            </div>
            <div class="main">
              <nav>
                <ul class="options">
                  <button class="hourly" @click="setActiveTab('hourly')" :class="{ active: activeTab === 'hourly' }">Por hora</button>
                  <button class="week" @click="setActiveTab('week')" :class="{ active: activeTab === 'week' }">A 5 dias</button>
                </ul>
                <ul class="options units">
                  <button class="celcius" @click="setActiveOp('celcius')" :class="{ active: activeOp === 'celcius' }">°C</button>
                  <button class="fahrenheit" @click="setActiveOp('fahrenheit')" :class="{ active: activeOp === 'fahrenheit' }">°F</button>
                </ul>
              </nav>

              <div class="cards" v-if="activeTab === 'hourly'">              
                  <div class="card" >
                    <h2  class="day-name">03:00</h2>
                    <div class="card-icon">
                      <img src="../assets/img/weather/03d.png" class="day-icon" alt="">
                    </div>
                    <div class="day-temp">
                      <h2 class="temp">24°C</h2>
                    </div>
                  </div>
              </div>

              <div class="cards" v-if="activeTab === 'week'">              
                  <div class="card" >
                    <h2  class="day-name">Lunes</h2>
                    <div class="card-icon">
                      <img src="../assets/img/weather/09d.png" class="day-icon" alt="">
                    </div>
                    <div class="day-temp">
                      <h2 class="temp">25.7°C</h2>
                    </div>
                  </div>
              </div>

              
                
              <div class="highlights">
                <h2 class="heading">Destacados de hoy</h2>
                <div class="cards">
                  <div class="card2">
                    <h4 class="card-heading">Sensacion Termica</h4>
                    <div class="content">
                      <p class="uv-index">25,7°C</p>
                    </div>
                  </div>
                  <div class="card2">
                    <h4 class="card-heading">Viento</h4>
                    <div class="content">
                      <p class="wind-speed">30.4 km/h</p>
                    </div>
                  </div>
                  <div class="card2">
                    <h4 class="card-heading">Maxima</h4>
                    <div class="content">
                      <p class="sun-rise">20.6 °C</p>
                    </div>
                  </div>
                  <div class="card2">
                    <h4 class="card-heading">Humedad</h4>
                    <div class="content">
                      <p class="humidity">30%</p>
                    </div>
                  </div>
                  <div class="card2">
                    <h4 class="card-heading">Presion</h4>
                    <div class="content">
                      <p class="visibilty">1016 hPa</p>
                    </div>
                  </div>
                  <div class="card2">
                    <h4 class="card-heading">Minima</h4>
                    <div class="content">
                      <p class="air-quality">16.7 °C</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
    
  </div>
</template>

<script>
import axios from 'axios';
import cityList from '../assets/city.list.json';


export default {
  name: 'HomeView',
  data() {
    return {
      activeTab: 'week',
      activeOp: 'celcius',
      selectedCity:'',
      searchQuery: '',
      searchResults: [],
    };
  },

  methods: {
    handleSelectionChange(event) {
                    const selectedIndex = event.target.selectedIndex;
                    const selectedOption = this.searchResults[selectedIndex];
                    const lat = selectedOption.lat;
                    const lon = selectedOption.lon;
                },
    async searchPlace(query) {
                    const baseUrl = "https://nominatim.openstreetmap.org/search";
                    const params = {
                        q: query,
                        format: "json",
                    };

                    try {
                        const response = await fetch(`${baseUrl}?${new URLSearchParams(params)}`);
                        const data = await response.json();
                        this.searchResults = data;
                    } catch (error) {
                        console.error(error);
                    }
                },
    setActiveTab(tab) {
      this.activeTab = tab;
    },
    setActiveOp(op) {
      this.activeOp = op;
      if (op === 'fahrenheit') {
        document.querySelector('.temp-unit').textContent = '°F';
      } else {
        document.querySelector('.temp-unit').textContent = '°C';
      }
    },
    async searchCityWeather(selectedCity) {
  try {
    const apiKey = 'NWP5A5DETM2C6BV4R9ZZR6FFT'; // Tu API Key
    const apiUrl = `https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/${selectedCity.name}?key=${apiKey}`;

    const response = await axios.get(apiUrl);
    // Procesa la respuesta y actualiza los datos del componente con la información del clima
    console.log(response.data);
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
},
  },
  watch: {
                searchQuery: function(newQuery) {
                    this.searchPlace(newQuery);
                }
            }
  
};
</script>


<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap");
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css");


:root {
  --primary-color: #5598fd;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

.home{
  display: flex;
  justify-content: center;
}
body {
  font-weight: bold;
  display: flex;
  justify-content: center;
  min-height: 100vh;
  min-width: 1000px;
  padding: 50px;
  background: var(--primary-color);
  background-color: #434343;
  background-size: cover;
  background-position: center;
  transition: background-image 0.3s ease;
}
img {
  width: 100%;
}
.wrapper {
  display: flex;
  width: 1200px;
  min-width: 900px;
  border-radius: 20px;
  overflow: hidden;
}
.sidebar {
  width: 30%;
  min-width: 250px;
  padding: 20px;
  background: #232323f0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: #fff;
}
.material-symbols-outlined{
    font-weight: bold;
    display: block;
}

.search {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 30px;
  margin-top: 20px;
  position: relative;
}
.search input {
  width: 100%;
  border-radius: 16px;
  height: 40px;
  background: #383838;
  font-weight: bold;
  border: 1px solid #383838;
  padding: 0 15px;
  font-size: 14px;
  color: #ffffff;
}
.search input:focus {
  outline: none;
  border: 1px solid var(--primary-color);
}
.search button {
  min-width: 40px;
  height: 40px;
  border: none;
  border-radius: 0px 16px 16px 0px;

  background: linear-gradient(168deg, #3d4ef3, #1c1ab5);
  color: #fff;
  font-size: 14px;
  cursor: pointer;
}
.search ul {
  max-height: 300px;
  overflow-y: auto;
  position: absolute;
  width: 100%;
  top: 40px;
  border-radius: 5px;
  transition: all 0.3s ease;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}
.search ul li {
  padding: 10px 15px;
  border-bottom: 1px solid #2e2e2e;
  cursor: pointer;
  background-color: #343434;
  color: white;
  text-transform: capitalize;
}
.search ul li:last-child {
  border-bottom: none;
}
.search ul li:hover {
    background-color: #343434;
}
.search ul li.active {
  background-color: #343434;
}
.weather-icon {
  width: 100%;
  height: 150px;
  text-align: center;
  margin-top: 20px;
  margin-bottom: 100px;
}
.weather-icon #icon {
  width: 100%;
  object-fit: cover;
}
.temperature {
  display: flex;
}
.temperature #temp {
  font-size: 70px;
  font-weight: bold;
  line-height: 1;
}
.temp-unit{
  font-weight: bold;
}
.temperature span {
  font-weight: bold;
  font-size: 40px;
  margin-top: -10px;
  display: block;
}
.divider {
  width: 100%;
  height: 1px;
  background: #e9ecef;
  margin: 20px 0;
}
.condition-rain {
  font-size: 12px;
  text-transform: capitalize;
}
.condition-rain div {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}
.condition-rain div i {
  width: 20px;
}
.location {
  display: flex;
  align-items: center;
  font-size: 14px;
  gap: 10px;
  margin-top: 10px;
  font-weight: bold;
}
.main {
  width: 100%;
  min-width: 400px;
  padding: 20px 40px;
  background-color: #232323f0;
  position: relative;
  padding-bottom: 90px;
}

.main nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.main nav .options {
  display: flex;
  gap: 20px;
  align-items: center;
}
.main nav .options button {
  border: none;
  background: none;
  font-size: 16px;
  padding: 8px;
  font-weight: 600;
  color: #ffffff;
  cursor: pointer;
  background-color: #575757;
  border-radius: 20px;
  text-transform: capitalize;
}
.main nav .options button.active {
  box-shadow: 5px 5px 5px -1px rgb(0 143 255 / 16%), -5px -5px 5px -1px rgb(48 84 255 / 39%);
  color: white;
  border-radius: 20px;
  background: linear-gradient(168deg, #3d4ef3, #1c1ab5);
  padding: 8px;
 
}

.main nav .units button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  color: #ffffff;
  background-color: #575757;
}
.main nav .units button.active {
  color: #ffffff; 
}
.main .cards {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 50px;
}

.cards .card {
  width: 87px;
  height: 130px;
  border-radius: 20px;
  color: #fff;
  background: linear-gradient(180deg, #515151, #333333);
  text-align: center;
  padding: 10px 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  
}

.card{
  width: 87px;
  height: 130px;
  border-radius: 20px;
  color: #fff;
  background: linear-gradient(180deg, #515151, #333333);
  text-align: center;
  padding: 10px 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card:hover{
    
    transition: 1s ease;
    cursor: pointer;
    background: linear-gradient(168deg, #3d4ef3, #1c1ab5);
    animation: changeColor 5s infinite alternate;
}
.card:not(:hover){
  transition: 1s ease;
  transform: scale(1);
}
.card h2 {
  font-size: 15px;
  font-weight: 600;
}
.card .card-icon {
  width: 50%;
  margin: 0 auto;
}
.card .day-temp {
  font-size: 12px;
  display: flex;
  justify-content: center;
  display: flex;
}
.highlights {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 50px;
}
.highlights .heading {
  width: 31%;
  color: white;
  font-size: 24px;
  margin-bottom: -34px;
  font-weight: 600;
  background: linear-gradient(180deg, #4bff5c, #00ab62);
  border-radius: 16px;
  padding-left: 14px;
}

.card2 {
  width: 250px;
  height: 150px;
  border-radius: 20px;
  color: #ffffff;
  background-color: #2a2a2a;
  padding: 10px 20px;
  display: flex;
  flex-direction: column;
}
.card2 .card-heading {
  color: #17ffb6;
  font-size: 21px;
  display: flex;
  justify-content: center;
}

.card2 .content {
  margin-top: 20px;
}
.card2 .content p:first-child {
  text-align: center;
  font-weight: bold;
  font-size: 30px;
}
.card2 .content p:nth-child(2) {
  font-size: 12px;
  margin-top: 20px;
  text-align: left;
}
.credits {
  text-align: center;
  font-size: 12px;
  color: #c2c2c2;
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
} 

.suggestions {
  list-style: none;
  padding: 0;
  margin-top: 5px;
  background-color: rgb(51, 51, 51);
  color: white;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: absolute;
  width: 13%;
  z-index: 1;
  max-height: 200px; 
  overflow-y: auto; 
  border:none;
}

.suggestions li {
  padding: 5px 10px;
  cursor: pointer;
  border: none;
}

.suggestions li:hover {
  background-color: #f1f1f1;
}

ul::-webkit-scrollbar {
      width: 10px;
    }
    
    ul::-webkit-scrollbar-track {
      background: #363636;
    }
    
    ul::-webkit-scrollbar-thumb {
      background: #888;
      border-radius: 5px;
    }
    
    ul::-webkit-scrollbar-thumb:hover {
      background: #555;
    }

.search select {
  position: absolute;
  width: 100%;
  height: 40px;
  top: 100%;
  left: 0;
  font-weight: bold;
  background-color: #383838;
  border: 1px solid #383838;
  border-top: none;
  font-size: 14px;
  padding: 0 15px;
  color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 1;
  margin-top: 8px;
}


  
  
    

#cardWeek{
  width: 117px;
  height: 185px;
}
</style>
