<script>
import axios from 'axios'
import { apiKey } from '../utils/apiKey'
import { linkLocationKey } from '../utils/linksAPI'
import WeatherNow from '@/components/city/WeatherNow.vue'
import DailyForecast from '@/components/city/DailyForecast.vue'
import TwelveHoursGraphic from '@/components/graphics/TwelveHoursGraphic.vue'
import LastDayGraphic from '@/components/graphics/LastDayGraphic.vue'
import FiveDaysGraphic from '@/components/graphics/FiveDaysGraphic.vue'

export default {
  data() {
    return {
      cityID: this.$route.query.key,
      found: false,
      city: {}
    }
  },
  methods: {
    getName() {
      axios
        .get(`${linkLocationKey}/${this.cityID}?apikey=${apiKey}&language=es`)
        .then((response) => {
          this.city = response.data
          this.found = true
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted() {
    this.getName()
  },
  components: { WeatherNow, DailyForecast, TwelveHoursGraphic, LastDayGraphic, FiveDaysGraphic }
}
</script>

<template>
  <div class="main" v-if="found">
    <div class="city">
      <strong>{{ city.LocalizedName }}</strong>
      <p1>{{ city.AdministrativeArea.LocalizedName }}, {{ city.Country.LocalizedName }}</p1>
    </div>
  </div>

  <div class="main">
    <div class="weather">
      <WeatherNow :cityID="cityID" />
      <DailyForecast :cityID="cityID" />
    </div>
  </div>

  <div class="main">
    <div class="city">
      <strong>Próximas 12 horas</strong>
    </div>
  </div>

  <div class="main">
    <TwelveHoursGraphic :cityID="cityID" />
  </div>

  <div class="main">
    <div class="city">
      <strong>Próximos 5 días</strong>
    </div>
  </div>

  <div class="main">
    <FiveDaysGraphic :cityID="cityID" />
  </div>

  <div class="main">
    <div class="city">
      <strong>Últimas 24 horas</strong>
    </div>
  </div>

  <div class="main">
    <LastDayGraphic :cityID="cityID" />
  </div>

</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

.main {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 15px 0;
  margin-bottom: 20px;
}

.city {
  width: 90%;
  border-bottom: 1px solid #aaaaaa;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
}

.city strong {
  font-family: 'Montserrat', sans-serif;
  font-size: 50px;
  font-weight: bold;
}

.city p1 {
  font-family: 'Poppins', sans-serif;
  font-weight: bold;
  font-size: 20px;
}

.weather {
  width: 80%;
  background-color: #f3f6f7;
  padding: 20px;
  border-radius: 25px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
</style>
