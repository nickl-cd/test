<template>
  <div class="about">
    <h1>This is a test page</h1>
    <input
      v-model="inputValue"
      @input="showDelayedSearchResults(inputValue)"
    />
  </div>
</template>

<script>
export default {
  name: 'Test',

  data () {
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
    ]
  },

  mounted () {
    console.log(this.cities)
  }

  methods: {
    showDelayedSearchResults (input) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          return this.cities.filter(city => city.toLowerCase() === input.toLowerCase())
        }, 1000)
      )
    }
  }
}
</script>
