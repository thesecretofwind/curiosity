<!--  -->
<template>
  <!--  搜索-->
  <div class="search">
    <div class="logo">
      <a href="/">
        <img src="../../static/images/logo-dark.png" alt="回家打开">
      </a>
    </div>
    <form id='search_form' action="javascript:;">
      <input type="search" placeholder="搜索的城市" v-model="city">
    </form>
    <div class="search-btn">
      <img src="../../static/images/search.png" @click="getCityId">
    </div>
  </div>
</template>

<script>
  /* eslint-disable */
  import axios from 'axios'
  import bus from '../bus'

  export default {
    data() {
      return {
        city: '',
        tempCity: '',
      };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {
      bus.$on('historyCity', data => {
        if (data == this.tempCity) {
          alert('该城市天气预报正在展示中')
        } else {
          this.tempCity = data
          this.getCityId()
        }
      })
      this.getCityId()
    },
    methods: {
      async getCityWeather(location = '101010100', city) {
        let cityQueryUrl =
          `https://devapi.heweather.net/v7/weather/now?location=${location}&key=cf74450d83214e5bbb314ee6719b406f`
        let d = await axios.get(cityQueryUrl)
        d.data.now.city = city;
        bus.$emit('weatherData', d.data.now)
      },
      async getCityId() {
        if (!this.tempCity) {
          this.tempCity = this.city === '' ? '北京' : this.city;
        }else{
          this.tempCity = this.city
        }
        let cityIdQuery =
          `https://geoapi.heweather.net/v2/city/lookup?location=${this.tempCity}&mode=fuzzy&key=cf74450d83214e5bbb314ee6719b406f`
        let d = await axios.get(cityIdQuery)
        if (d.status == "200") {
          this.getCityWeather(d.data.location[0].id, this.tempCity)
          this.getFutureWeather(d.data.location[0].id)
          this.getComfort(d.data.location[0].id)
          bus.$emit('city', this.tempCity)
        } else {
          alert('您输入的城市不存在或暂未收录')
        }
      },
      async getFutureWeather(location = '101010100') {
        let futureQuery =
          `https://devapi.heweather.net/v7/weather/3d?location=${location}&key=cf74450d83214e5bbb314ee6719b406f`
        let d = await axios.get(futureQuery)
        bus.$emit('futureWeather', d.data.daily)
      },
      async getComfort(location = '101010100') {
        let cityComfortQuery =
          `https://devapi.heweather.net/v7/indices/1d?type=1,2,3,5,6,8,9,10&location=${location}&key=cf74450d83214e5bbb314ee6719b406f`
        let d = await axios.get(cityComfortQuery)
        bus.$emit('daily', d.data.daily)
      }
    },


  };
</script>
<style scoped>
  /* @import url(); 引入css类 */
  /* 搜索 */
  .search {
    width: 100%;
    height: 0.5rem;
    background-color: rgba(0, 0, 0, 0);
    display: flex;
    justify-content: space-evenly;
    align-items: center;

  }

  #search_form {
    width: 2.8rem;
    height: 0.4rem;
    position: relative;
    margin-left: .1rem;
  }

  .logo a img {
    width: 1rem;
  }
  .search #search_form>input {
    width: 2rem;
    height: 0.4rem;
    border-radius: 0.1rem;
    text-indent: 0.1rem;
    outline: none;
    position: absolute;
    border: none;
    border-bottom: 0.01rem solid white;
    background-color: rgb(255, 255, 255, 0.05);
    color: white;
  }

  .search .search-btn {
    width: 0.5rem;
    position: relative;
    padding-right: .2rem;
  }

  .search .search-btn img {
    width: 0.25rem;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>