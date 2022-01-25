<template>
  <div class="about">
    <h1>This is a test page</h1>

    <!-- CITY SEARCH -->
    <div>
      <h2>City Search</h2>
      <input
        v-model="citySearch"
        @input="showDelayedSearchResults(citySearch)"
      />

      <ul>
        <li
          v-for="(city, index) in filteredCities"
          :key="index"
        >
          {{ city }}
        </li>
        <span
          class="error"
          v-if="filteredCitiesErrorMessage"
        >
          {{ filteredCitiesErrorMessage }}
        </span>
      </ul>
    </div>

    <!-- TODOS -->
    <div>
      <h2>Todos</h2>
      <input v-model="todo.title"/>

      <span
        class="error"
        v-if="todoErrorMessage"
      >
        {{ todoErrorMessage }}
      </span>

      <div class="todos">
        <div
          :class="['todos__todo', todo.completed ? 'todos__todo--completed' : 'todos__todo--incomplete']"
          v-for="(todo, index) in todos"
          :key="index"
          @click="changeTodoComplete(todo)"
        >
          <p>{{ todo.userId }}</p>
          <p>{{ todo.id }}</p>
          <p>{{ todo.title }}</p>
          <p>{{ todo.completed }}</p>
        </div>
      </div>

      <button @click="addToDo">Add ToDo</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Test',

  data () {
    return {
      citySearch: "",
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
      filteredCitiesErrorMessage: "",
      todos: [],
      todo: {
        userId: null,
        id: null,
        title: '',
        completed: false
      },
      todoErrorMessage: ""
    }
  },

  async mounted () {
      await axios.get('https://jsonplaceholder.typicode.com/todos')
      .then((res) => {
        this.todos = res.data.slice(0, 20)
        console.table(this.todos)
      })
      .catch(err => console.error(err))
  },

  methods: {
    showDelayedSearchResults (input) {
      return new Promise(resolve => {
        setTimeout(() => {
          this.filteredCities = this.cities.filter(city => city.toLowerCase().includes(input.toLowerCase()))
          if (this.filteredCities.length === 0) {
            this.filteredCitiesErrorMessage = "No search results found."
          } else {
            this.filteredCitiesErrorMessage = ""
            resolve(this.filteredCities)
            console.log(this.filteredCities)
          }
        }, 500)
      })
    },

    addToDo () {
      const { title, completed } = this.todo
      const newToDo = {
        userId: Math.floor(Math.random() * 10) + 1,
        id: Math.floor(Math.random() * 10) + 1,
        title,
        completed
      }
      if (title) {
        this.todos.push(newToDo)
        this.todo.title = '',
        this.todoErrorMessage = ''
        console.table(this.todos)
      } else {
        this.todoErrorMessage = 'All fields required.'
      }
    },

    changeTodoComplete (todo) {
      console.log(todo)
      todo.completed = !todo.completed
    }
  }
}
</script>

<style lang='scss'>
ul {
  margin: 0;
  padding: 0;
}
.error {
  color: #FF9494;
  display: block;
}

.todos {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  align-items: center;

  &__todo {
    margin: 10px;
    padding: 10px;
    min-width: 200px;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    &--completed {
      background-color: #42b983;
      transition: 0.1s ease-in-out;
      &:hover {
        background-color: #5dc596;
      }
    }
    &--incomplete {
      background-color: #FFFF88;
      transition: 0.1s ease-in-out;
      &:hover {
        background-color: #ffffc3;
      }
    }
  }
}
</style>
