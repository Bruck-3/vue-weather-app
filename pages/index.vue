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

          <v-img src="https://via.placeholder.com/300x300"></v-img>
          <p class="text-center text-h6">{{ cityName }}</p>
          <p class="text-center text-subtitle-1">{{ weatherCondition }}</p>
          <p class="text-center text-h4">{{ temperature }}&deg;C</p>
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
      cityName: 'City Name',
      weatherCondition: 'Weather Condtion',
      temperature: 'TEMP',
      apikey: 'FXC7yeBIR5XvzlEUMg09xxgzcbs2we4J',
    }
  },
  methods: {
    clickLocation() {
      let location = this.input.trim()
    },
  },
}
</script>
