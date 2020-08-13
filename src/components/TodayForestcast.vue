<!--  -->
<template>
  <!-- 今日天气 -->
  <div class="infomation" >
    <div class="now">
      <div class="now-city-time">
        <span class="city">{{city}}</span>
        <span>{{time| timeTransform}}</span>
      </div>
      <div class="situation">
        <img :src="imgUrl" alt="">
        <h1 class="text">{{weather}}</h1> 
        <div class="temp">
          <h3 class="tmp">温度:{{temp}}</h3> 
          <h3 class="fl">体感温度:{{fl}}</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import bus from '../bus'
export default {
  data() {
    return {
        data:null,
        city:'',
        time:'',
        imgUrl:'',
        weather:'',
        temp:'',
        fl:''
    };
  },
  //生命周期 - 创建完成（访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {
      bus.$on('weatherData',data => {
             this.data = data
            this.city = data.city
            this.time = data.obsTime
            this.imgUrl = require(`../../static/icon/${data.icon}.png`)
            this.weather = data.text
            this.temp = data.temp
            this.fl = data.feelsLike
          })
  },
  filters:{
    timeTransform(value) {
        let d = new Date(value);
        let year = d.getFullYear(),
          month = d.getMonth() + 1,
          day = d.getDate();
        if (month < 10) {
          month = "0" + month;
        }
        if (day < 10) {
          day = "0" + day;
        }
        return `${year}-${month}-${day}  ${d.getHours()} :${d.getMinutes()}`
      }
  }
};
</script>
<style scoped>
/* @import url(); 引入css类 */
/* 今日天气 */
.now{
  padding-top: 0.2rem;
  display: flex;
  flex-direction: column;
  background: linear-gradient(45deg, #353F4E, #1C1D1F);
}
.now .now-city-time {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.now .city{
  font-size: .4rem;
  /* text-align: center; */
}
.now .situation {
  padding-top: 0.2rem;
  display: flex;
  justify-content: space-evenly;
  font-size: .15rem;
  align-items: center;
}
.now .situation img {
  width: 1rem;
  height: 1rem;
  vertical-align: bottom;
}
.now .temp{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.now .temp>h3 {
  font-size: 0.2rem;
  margin-top: 0.1rem;
}
</style>