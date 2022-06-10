<template>
  <div>
    <div class="buttonContent">
      <button @click="bouttonAfficherPrevision">PREVISION METEO</button>
      <button @click="whereAmI">GEOLOCALISATION</button>
    </div>

    <p v-if="showCoords">
      Votre latitude: {{ this.latitude }}. Votre longitude:
      {{ this.longitude }}. Avec une précision de: {{ this.accuracy }}
    </p>

    <div>
      <ul v-if="showMeteo" class="contentInformations">
        <li v-for="element in donnees" :key="element.dt">
          <div class="dateContent">
            <p class="date">{{ element.dt_txt }}</p>
          </div>
          <div class="paragraph">
            <p>Température minimum: {{ element.main.temp_min }}°</p>
            <p>Température maximum: {{ element.main.temp_max }}°</p>
            <p>Vitesse du vent: {{ element.wind.speed }} Km/h</p>
            <p>Description: {{ element.weather[0].description }}</p>
          </div>
          <img
            class="weatherLogo"
            :src="
              'http://openweathermap.org/img/wn/' +
              element.weather[0].icon +
              '@2x.png'
            "
            alt="weather"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      donnees: [],
      showMeteo: false,
      showCoords: false,
      latitude: 0,
      longitude: 0,
      accuracy: 0,
      linkApi:
        "'https://api.openweathermap.org/data/2.5/forecast?lat='+{{this.latitude}}+'&lon=7.2743404&units=metric&appid=e16c9d2fe603523108833f2dba5ea92f'",
    };
  },

  methods: {
    async bouttonAfficherPrevision() {
      if (!this.showMeteo) {
        this.showMeteo = true;
      } else {
        this.showMeteo = false;
      }

      let prevision = await fetch(
        "https://api.openweathermap.org/data/2.5/forecast?lat=" +
          this.latitude +
          "&lon=" +
          this.longitude +
          "&units=metric&appid=e16c9d2fe603523108833f2dba5ea92f"
      );

      let donnees = await prevision.json();
      console.log(donnees);
      this.donnees = donnees.list;
    },

    whereAmI() {
      if (!this.showCoords) {
        this.showCoords = true;
      } else {
        this.showCoords = false;
      }

      navigator.geolocation.getCurrentPosition((position) => {
        this.latitude = position.coords.latitude;
        this.longitude = position.coords.longitude;
        this.accuracy = position.coords.accuracy;
      });
    },
  },
};
</script>

<style scoped>
.contentInformations {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  text-align: center;
  list-style-type: none;
  padding-right: 40px;
}

.contentInformations li {
  background-image: url(../assets/paysage-app.png);

  height: 386px;
  width: 284px;
  margin: 10px;
  border-radius: 30px;
}

.contentInformations p {
  font-size: 20px;
  margin-bottom: 0px;
}

.date {
  background-color: white;
  border-radius: 50px;
  margin-left: 70px;
  margin-right: 70px;
  padding-top: 10px;
  padding-bottom: 10px;
}

.buttonContent {
  text-align: center;
}

.buttonContent button {
  padding: 8px 50px;
  margin: 20px 4rem;
  margin-top: 50px;
  background-color: white;
  color: rgb(49 149 2);
  border: 0px;
  border-radius: 20px;
  font-size: 20px;
}

.buttonContent button:hover {
  box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset,
    rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;
  border: 1px solid black;
}
</style>
