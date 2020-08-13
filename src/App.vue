<template>
  <div id="app">
    <Explore></Explore>
    <History></History>
    <TodayForecast></TodayForecast>
    <FutureDays></FutureDays>
    <LifeRate></LifeRate>
  </div>
</template>

<script>
  import Explore from './components/Explore'
  import History from './components/History'
  import TodayForecast from './components/TodayForestcast'
  import FutureDays from './components/FutureDays'
  import LifeRate from './components/LifeRate'
  export default {
    name: 'App',
    components: {
      Explore,
      History,
      TodayForecast,
      FutureDays,
      LifeRate
    }
  }
</script>

<style>
  * {
    padding: 0;
    margin: 0;
  }

  ul,
  li {
    list-style: none;
  }

  body {
    background-size: 120%;
    background-color: rgba(0, 0, 0, 0.2);
    color: #fff;
  }
</style>