<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card width="400px">
          <v-card-text class="text-center text-h4"> Weather App</v-card-text>
          <v-form @submit.prevent="clickLocation">
            <v-text-field
              outlined
              v-model="input"
              label="Enter Location"
            ></v-text-field>
          </v-form>

          <div v-if="showDetails">
            <v-img
              max-height="200"
              max-width="300"
              src="../assets/images/day.svg"
            ></v-img>
            <div class="text-center">
              <v-icon class="icon-thing">mdi-face</v-icon>
            </div>

            <p class="text-center text-h6">{{ weatherDetails.cityName }}</p>
            <p class="text-center text-subtitle-1">
              {{ weatherDetails.weatherCondition }}
            </p>
            <p class="text-center text-h4">
              {{ weatherDetails.temperature }}&deg;C
            </p>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      input: null,
      showDetails: false,
      image: '',
      weatherDetails: {
        cityName: '',
        weatherCondition: '',
        temperature: '',
      },
      apiKey: 'FXC7yeBIR5XvzlEUMg09xxgzcbs2we4J',
    }
  },
  methods: {
    clickLocation() {
      let location = this.input.trim()
      this.getCityKey(location)
    },
    getCityKey(location) {
      const base =
        'http://dataservice.accuweather.com/locations/v1/cities/search?'

      this.$axios
        .get(base + `apikey=${this.apiKey}&q=${location}`)
        .then((response) => {
          if (response.data[0]) {
            this.weatherDetails.cityName =
              response.data[0].AdministrativeArea.EnglishName
            this.getCityWeather(response.data[0].Key)
            this.input = null
          } else {
            console.log("Couldn't find The city You are looking for")
          }
        })
        .catch((error) => {
          console.log(error)
        })
    },
    getCityWeather(key) {
      const base = `http://dataservice.accuweather.com/forecasts/v1/hourly/1hour/${key}?`
      this.$axios
        .get(base + `apikey=${this.apiKey}`)
        .then((response) => {
          if (response.data[0].IsDayLight) {
            this.img = '../assets/images/day.svg'
          } else {
            this.img = '../assets/images/night.svg'
          }
          this.weatherDetails.weatherCondition = response.data[0].IconPhrase
          this.weatherDetails.temperature =
            response.data[0].Temperature.UnitType
          this.showDetails = true
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>
<style lang="css">
.icon-thing {
  position: relative;
  width: 100px;
  top: -30px;
}
</style>