<template>
  <div id="app">
    <Schedule :team="team"/>
  </div>
</template>

<script>
import Schedule from './components/Schedule';

export default {
  name: 'app',
  components: {
    Schedule
  },
  data() {
    return {
      team: [
        {
          name: "Севера Андрій"
        },
        {
          name: "Шпікула Олег"
        },
        {
          name: "Пиженков Віталій"
        },
        {
          name: "Кузьмін Євген"
        },
        {
          name: "Байда Анатолій"
        },
        {
          name: "Томкевич Остап"
        },
        {
          name: "Сокульський Влад"
        }
      ]
    }
  }
}
</script>

<style lang="scss">
body, html {
  margin: 0;
  padding: 0;
  background-color: #2c3e50;
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #bdc3c7;
}
</style>
