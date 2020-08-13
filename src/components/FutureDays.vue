<!--  -->
<template>
  <!-- 未来三天天气预报 -->
  <div class="forecast">
    <!-- 未来的天气状况 动态获取 -->
    <div class="nowday forecast-item">
      <div class="forecast-situation">
        <img :src="imgUrl[0]" alt="">
        今天 * <span class="txt">{{weather[0]}}</span>
      </div>
      <div class="forecast-temp">
        <span class="min">{{minTemp[0]}}℃-</span>
        <span class="max">{{maxTemp[0]}}℃</span>
      </div>
    </div>
    <div class="nowday forecast-item">
      <div class="forecast-situation">
        <img :src="imgUrl[1]" alt="">
        明天 * <span class="txt">{{weather[1]}}</span>
      </div>
      <div class="forecast-temp">
        <span class="min">{{minTemp[1]}}℃-</span>
        <span class="max">{{maxTemp[1]}}℃</span>
      </div>
    </div>
    <div class="nowday forecast-item">
      <div class="forecast-situation">
        <img :src="imgUrl[2]" alt="">
        后天 * <span class="txt">{{weather[2]}}</span>
      </div>
      <div class="forecast-temp">
        <span class="min">{{minTemp[2]}}℃-</span>
        <span class="max">{{maxTemp[2]}}℃</span>
      </div>
    </div>
  </div>
</template>

<script>
  import bus from '../bus'
  export default {
    data() {
      return {
        weather: [],
        imgUrl: [],
        maxTemp: [],
        minTemp: []
      };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {
      bus.$on('futureWeather', data => {
        data.forEach(item => {
          this.weather.push(item.textDay)
          this.imgUrl.push(require(`../../static/icon/${item.iconDay}.png`))
          this.maxTemp.push(item.tempMax)
          this.minTemp.push(item.tempMin)
        })
      })
    }
  };
</script>
<style scoped>
  /* @import url(); 引入css类 */
  /* 未来天气状态 */
  .forecast {
    background-color: rgba(0, 0, 0, 0.2);
    margin: 0 0.2rem;
    border-radius: 0.1rem;
    margin-left: 1px;
  }

  .forecast-item {
    height: .3rem;
    display: flex;
    justify-content: space-between;
    margin: .1rem .2rem 0;
    padding-top: 0.1rem;
  }

  .forecast-item:last-of-type {
    padding-bottom: 0.1rem;
  }

  .forecast-item .forecast-situation>img {
    width: 0.2rem;
    height: 0.2rem;
    vertical-align: middle;
  }

  .forecast-item .forecast-situation {
    font-size: 0.16rem;
  }

  .forecast-temp {
    font-size: 0.18rem;
  }

  .lifestyle {
    /* display: none; */
    margin: 0 0.2rem;
    background-color: rgba(0, 0, 0, 0.2);
    border-radius: 0.1rem;
    padding: 0 0.2rem 0.2rem 0.2rem;
  }

  .lifestyle>h2 {
    text-align: center;
    margin-top: 0.2rem;
    font-size: .2rem;
    padding-top: .15rem;
  }

  .lifestyle .lifestyle-box {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
  }

  .lifestyle .lifestyle-box .lifestyle-item {
    display: flex;
    flex-direction: column;
    width: 0.7rem;
    height: 0.4rem;
    font-size: 0.14rem;
    text-align: center;
    padding-top: 0.1rem;
    padding-bottom: 0.05rem;

  }

  .lifestyle .lifestyle-box .lifestyle-item>i {
    font-size: 0.2rem;
  }

  .lifestyle .lifestyle-box .lifestyle-item>span {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>