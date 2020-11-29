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
            <v-img :src="image"></v-img>
            <div class="text-center">
              <v-icon size="50">mdi-cloud</v-icon>
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
      icon: '',
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
          const cityResponse = response.data[0]
          if (cityResponse) {
            this.weatherDetails.cityName =
              cityResponse.AdministrativeArea.EnglishName
            this.getCityWeather(cityResponse.Key)
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
          const cityWeather = response.data[0]

          if (cityWeather.IsDaylight == true) {
            this.image = 'day.svg'
          } else if (!cityWeather.IsDaylight) {
            this.image = 'night.svg'
          }
          this.i
          this.weatherDetails.weatherCondition = cityWeather.IconPhrase
          this.weatherDetails.temperature = cityWeather.Temperature.UnitType
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