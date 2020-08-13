<!--  -->
<template>
  <div class="liftRate">
    <!-- 生活指数 -->
    <div class="lifestyle">
      <h2>生活指数</h2>
      <div class="lifestyle-box" @click="rowclick">
        <div class="lifestyle-item" index="0">
          <i class="iconfont icon-iconset0451" index="0"></i>
          <span index="5">紫外线指数</span>
        </div>
        <div class="lifestyle-item" index="1">
          <i class="iconfont icon-xiche" index="1"></i>
          <span index="1">洗车指数</span>
        </div>
        <div class="lifestyle-item" index="2">
          <i class="iconfont icon-lvyou" index="2"></i>
          <span index="2">旅游指数</span>
        </div>
        <div class="lifestyle-item" index="3">
          <i class="iconfont icon-shushidu" index="3"></i>
          <span index="3">舒适度指数</span>
        </div>

        <div class="lifestyle-item" index="4">
          <i class="iconfont icon-kongqizhiliangjiance" index="4"></i>
          <span index="4">空气污染扩散条件指数</span>
        </div>
        <div class="lifestyle-item" index="5">
          <i class="iconfont icon-yundong" index="5"></i>
          <span index="5">运动指数</span>
        </div>
        <div class="lifestyle-item" index="6">
          <i class="iconfont icon-3chuanyixiguan" index="6"></i>
          <span index="6">穿衣指数</span>
        </div>
        <div class="lifestyle-item" index="7">
          <i class="iconfont icon-ganmaoyaowu-copy" index="7"></i>
          <span index="7">感冒指数</span>
        </div>
      </div>
    </div>
    <!-- 生活指数弹窗 -->
    <div class="lifestyle-tc" v-show="isShow">
      <h3>{{name |cutDown}}<span>{{category}}</span></h3>
      <p>{{text}}</p>
      <button @click="close">确定</button>
    </div>
  </div>

</template>

<script>
  /* eslint-disable */
  // https://devapi.heweather.net/v7/indices/1d?type=1,2,3,5,6,8,9,10&location=101010100&key=cf74450d83214e5bbb314ee6719b406f
  import bus from '../bus'
  export default {
    data() {
      return {
        data: [],
        name: '',
        category: '',
        text: '',
        isShow: false
      };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {
      bus.$on('daily', data => {
        data.forEach(item => {
          this.data.push(item)
        })
      })
    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {

    },
    methods: {
      rowclick(e) {
        let index = e.target.getAttribute('index')
        this.name = this.data[index].name
        this.category = this.data[index].category
        this.text = this.data[index].text
        this.isShow = true
      },
      close() {
        this.isShow = false
      }
    },
    filters: {
      cutDown(value) {
        if (value.length > 5) {
          return '空气污染指数'
        } else {
          return value
        }
      }
    }
  };
</script>
<style scoped>
  /* @import url(); 引入css类 */

  @import url(https://at.alicdn.com/t/font_681041_b16k75oo82a.css);

  .liftRate {
    margin-left: -19px;
    ;
  }

  /* 生活指数弹窗 */
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

  /* 生活指数弹窗 */
  .lifestyle-tc {

    position: fixed;
    width: 2rem;
    height: 2rem;
    bottom: 1.5rem;
    left: 50%;
    color: rgb(40, 45, 53);
    background-color: #fff;
    font-family: 幼圆;
    line-height: 0.25rem;
    ;
    border-radius: 0.15rem;
    transition-duration: 0.5s;
    transform: translateX(-50%);

  }

  .lifestyle-tc h3 {
    padding: .1rem;
  }

  .lifestyle-tc span {
    font-size: 0.15rem;
    font-weight: 700;
    text-indent: 0.16rem;
    margin-left: .3rem;
  }

  .lifestyle-tc p {
    text-indent: 0.3rem;
    font-size: 0.15rem;
    padding: .1rem;
  }

  .lifestyle-tc button {
    margin-left: 149px;
    border-radius: 6px;
    outline: none;
    width: 43px;
    padding: 5px;
    border-color: transparent;
    background: #fff;
    color: rgb(205, 45, 103);
  }

  /* .lifestyle-tc .fanghui {
  width: 0.3rem;
  height: 0.3rem;
  position: absolute;
  left: 0.1rem;
  top: 0.1rem;
}

.lifestyle-tc .fanghui>img {
  width: 100%;
}

.lifestyle-tc>h2 {
  font-size: 0.3rem;
  width: 2.2rem;
  margin: 0.3rem auto 0;
  text-align: center;
}

.lifestyle-tc>span {
  width: 100%;
  font-size: 0.2rem;
  margin-top: 1.5rem;
  display: block;
  font-weight: 700;
  text-indent: 0.16rem;
}
.lifestyle-tc>p {
  text-indent: 0.32rem;
  font-size: 0.2rem;
} */
</style>