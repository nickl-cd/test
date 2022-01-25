<template>
  <div class="about">
    <h1>This is a test page</h1>
    <input
      v-model="inputValue"
      @input="showDelayedSearchResults(inputValue)"
    />
    <ul>
      <li
        v-for="(city, index) in filteredCities"
        :key="index"
      >
        {{ city }}
      </li>
      <span>
        {{ this.errorMessage }}
      </span>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Test',

  data () {
    return {
      inputValue: "",
      cities: [
        "Aberdeen",
        "Abilene",
        "Akron",
        "Albany",
        "Albuquerque",
        "Alexandria",
        "Allentown",
        "Amarillo",
        "Anaheim",
        "Anchorage",
        "Ann Arbor",
        "Antioch",
        "Apple Valley",
        "Appleton",
        "Arlington",
        "Arvada",
        "Asheville",
        "Lacey",
        "Lafayette",
        "Lake Charles"
      ],
      filteredCities: [],
      errorMessage: ""
    }
  },

  mounted () {
    console.log(this.cities)
  },

  methods: {
    showDelayedSearchResults (input) {
      return new Promise(resolve => {
        setTimeout(() => {
          this.filteredCities = this.cities.filter(city => city.toLowerCase().includes(input.toLowerCase()))
          if (this.filteredCities.length === 0) {
            this.errorMessage = "No search results found."
          } else {
            this.errorMessage = ""
            resolve(this.filteredCities)
            console.log(this.filteredCities)
          }
        }, 500)
      })
    }
  }
}
</script>
