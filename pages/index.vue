<template>
  <div class="index">
    <!-- 在nuxt的框架中不需要配置路由 -->
    <el-carousel>
      <el-carousel-item v-for="(value,index) in banners" :key="index">
                                          <!-- 并集地址 需要完整的地址 -->
        <img class="banners_img" :src="`${$axios.defaults.baseURL}${value.url}`" alt />
      </el-carousel-item>
    </el-carousel>
    <!-- 搜索框 -->
    <div class="banner-content">
      <div class="search-bar">
        <!-- tab栏 -->
        <el-row type="flex" class="search-tab">
          <span
            v-for="(value,index) in options"
            :key="index"
            @click="dispose(index)"
            :class="{active: index == numbers}"
          >
            <i>{{value.name}}</i>
          </span>
        </el-row>

        <!-- 输入框 -->
        <el-row type="flex" align="middle" class="search-input" >
          <!-- 输入框实现提示文字                               获取用户输入的数据     回车跳转   -->        
          <input :placeholder="options[numbers].placeholder" v-model="uservalue" @keydown.enter="skip"/>
          <i class="el-icon-search" @click="skip"></i>
        </el-row>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      banners: [],
      options: [
        // 搜索框tab选项
        {
          name: "攻略",
          placeholder: "搜索城市",
          pageUrl: "/post?city=",
        },
        {
          name: "酒店",
          placeholder: "请输入城市搜索酒店",
          pageUrl: "/hotel?city=",
        },
        {
          name: "机票",
          placeholder: "请输入出发地",
          pageUrl: "/air",
        },
      ],
      numbers: 0,
      uservalue:'',
    };
  },
  async mounted() {
    // 調用dxios
    await this.$axios({
      url: "/scenics/banners",
    }).then((res) => {
      console.log(res);
      this.banners = res.data.data;
    });
  },
  methods: {
    // tab
    dispose(index) {
          // 实现点击的时候切换高亮
        this.numbers = index;
    if(this.options[this.numbers].name == '机票'){
       this.$router.push('/air')
    }
      },
     
     // 搜索跳转
      skip(){
         let res = this.options[this.numbers].pageUrl 
            //  并且把用户输入的参数带过去
        this.$router.push(res + this.uservalue)
    
      }
  }
};
</script>

<style lang="less" >
.index {
  color: blue;
  /deep/.el-carousel__container {
    height: 700px;
  }
  .banners_img {
    height: 100%;
    width: 100%;
  }
  .banner-content {
    z-index: 9;
    width: 1000px;
    position: absolute;
    left: 50%;
    top: 45%;
    margin-left: -500px;
    border-top: 1px transparent solid;

    .search-bar {
      width: 552px;
      margin: 0 auto;
    }

    .search-tab {
      // 高亮的样式
      .active {
        i {
          color: #333;
        }
        &::after {
          background: #eee;
        }
      }

      span {
        width: 82px;
        height: 36px;
        display: block;
        position: relative;
        margin-right: 8px;
        cursor: pointer;

        i {
          position: absolute;
          z-index: 2;
          display: block;
          width: 100%;
          height: 100%;
          line-height: 30px;
          text-align: center;
          color: #fff;
        }

        &:after {
          position: absolute;
          left: 0;
          top: 0;
          display: block;
          content: "";
          width: 100%;
          height: 100%;
          border: 1px rgba(255, 255, 255, 0.2) solid;
          border-bottom: none;
          transform: scale(1.1, 1.3) perspective(0.7em) rotateX(2.2deg);
          transform-origin: bottom left;
          background: rgba(0, 0, 0, 0.5);
          border-radius: 1px 2px 0 0;
          box-sizing: border-box;
        }
      }
    }

    .search-input {
      width: 550px;
      height: 46px;
      background: #fff;
      border-radius: 0 4px 4px 4px;
      border: 1px rgba(255, 255, 255, 0.2) solid;
      border-top: none;
      box-sizing: unset;

      input {
        flex: 1;
        height: 20px;
        padding: 13px 15px;
        outline: none;
        border: 0;
        font-size: 16px;
      }

      .el-icon-search {
        cursor: pointer;
        font-size: 22px;
        padding: 0 10px;
        font-weight: bold;
      }
    }
  }
}
</style>
