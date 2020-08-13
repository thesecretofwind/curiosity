<!--  -->
<template>
  <!-- 历史查询 -->
  <div class="history">
    <div class="la">
      <span>历史查询</span>
      <img src="../../static/images/more.png" alt="" @click="showHistory">
    </div>
    <div class="historied" v-show="isShow">
      <!-- 历史记录 -->
      <div class="history-item" v-for="(item,index) in historyArr" :key="index" :index="index">
        <span class="history-time" @click="historyExplore(item.city)">{{item.city}}</span>
        <span class="history-city">{{item.createdAt| timeTransform}}</span>
        <i class="iconfont icon-lajitong" @click="deleteHistory(item.id,index)"></i>
      </div>
    </div>
    <div class="more" v-show="isShow">...</div>
    <!-- 清除历史记录 -->
    <!-- <div class="clearBtn" v-show="isShow"> 
      清除历史记录
    </div> -->
  </div>


</template>

<script>
  import bus from '../bus'
  const AV = require("leancloud-storage");
  AV.init({
    appId: "qczamOv6BiTPIkvCXjT75CLi-gzGzoHsz",
    appKey: "VTGps47ckIj7153MdLIt9OCR",
    serverURL: "https://qczamov6.lc-cn-n1-shared.com"
  });
  // 声明 class
  const Todo = AV.Object.extend('weather');

  // 构建对象
  const todo = new Todo()
  const Query = new AV.Query("weather");
  export default {
    data() {
      return {
        isShow: false,
        historyArr: [],
        isInclude: false,
        showAll:true,
        tempArr:[]
      }
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {
      bus.$on('city', data => {
        // console.log(data)
        this.historyArr.forEach(item => {
          console.log(item.city,data)
          if (item.city == data) {
            //  console.log('相等')
            this.isInclude = true

          }
        })
        if (!this.isInclude){
          // console.log('不相等')
          todo.set('city', data)
          todo.save().then((todo) => {
            // 成功保存之后，执行其他逻辑
            alert(`保存成功。objectId：${todo.id}`);
          }, (error) => {
            // 异常处理
            console.log(error)
          });
        }
      })
    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {

      Query.find().then((data) => {
        data.forEach(item => {
          item.attributes.id = item.id
          item.attributes.createdAt = item.createdAt
          this.historyArr.push(item.attributes)
        })
      });
    },
    computed: {
      
    },
    methods: {
      showHistory() {
        this.isShow = !this.isShow
      },
      deleteHistory(id, index) {
        if (confirm('确定删除该历史记录?')) {
          this.historyArr.splice(index, 1)
          this.$forceUpdate();
          const todo = AV.Object.createWithoutData('weather', id);
          todo.destroy();
        }
      },
      historyExplore(city) {
        bus.$emit('historyCity', city)
      },
    },
    filters: {
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
  }
</script>
<style scoped>
  /* @import url(); 引入css类 */
  /* 历史查询 */
  @import url(http://at.alicdn.com/t/font_1733729_f0y2u3fs48g.css);

  .history {
    /* margin-top: 0.9rem; */
    overflow: auto;
    background-color: rgba(255, 255, 255, 0.05);
    transition-duration: 0.5s;
  }

  .history .la {
    display: flex;
    justify-content: space-between;
    height: .3rem;
    top: .45rem;
    width: 90%;
    font-size: .16rem;
    border-radius: .01rem solid #fff;
    margin: .1rem .2rem;
    line-height: .3rem;
  }

  .history .la span {
    color: #fff;
  }

  .history .la img {
    width: .2rem;
    height: .2rem;
    padding: .03rem;
    border: .01rem solid #cccc;
    border-radius: .05rem;
  }

  /* 历史记录 */
  .historied {
    overflow: hidden;
  }

  .history .history-item {
    display: flex;
    height: 40px;
    border-bottom: .01rem dashed #ccc;
    align-items: center;
    justify-content: space-evenly;
  }

  .history .history-item .history-time {
    font-size: .14rem;
  }

  .history .history-item .history-city {
    font-size: .18rem;
  }

  /* 清除历史记录 */
  .clearBtn {
    height: .3rem;
    text-align: center;
    text-decoration: underline;
    font-size: .2rem;
    margin-top: .1rem;
    line-height: .3rem;
  }

  .more {
    text-align: center;
    font-size: .3rem;
    margin-top: -0.13rem;
  }
</style>