<template>
  <div id="app">
    <span @click="toggleLang(lang)" :key="lang" v-for="lang in langs" :class="{'lang--active': lang === currentLang}" class="lang">
      {{lang}}
    </span>
    <Calendar :lang="currentLang" :currentDate="newDate"/>
    <form @submit.prevent="changeNewDate">
      <input v-model="date">
      <button>Сменить дату</button>
    </form>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'

export default {
  name: 'App',
  components: {
    Calendar
  },
  data() {
    return {
      date: '',
      newDate: '',
      langs: ['Ru', 'En'],
      currentLang: 'Ru'
    }
  },
  methods: {
    toggleLang(lang) {
      this.currentLang = lang
    },
    changeNewDate() {
      this.newDate = this.date
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.lang {
  cursor: pointer;
}
.lang--active {
  color: green;
}
</style>
