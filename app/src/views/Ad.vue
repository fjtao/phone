<template>
  <!--广告管理页面-->
  <div class="ad">
    <!--引入的公用头部导航栏-->
    <my-header2>
      <!--插槽修改导航栏中的标题文字-->
      <div slot="title">广告管理</div>
    </my-header2>
    <!--页面部分-->
    <div class="top">
      <div @click="display" :class="{active:num==1}">广告列表</div>
      <div @click="add" :class="{active:num==2}">添加广告</div>
    </div>
    <!--广告列表下的页面-->
    <div v-show="show">
      <!--拨号页面广告-->
      <div class="middle" @click="tone">
        <div class="font">拨号页面广告</div>
        <div
          v-if="unflod"
          class="iconfont"
          style="font-size: 0.8rem; color: #ddd;margin-right: 0.3rem;line-height: 1rem"
        >&#xe76a;</div>
        <div
          v-else
          class="iconfont"
          style="font-size: 0.8rem; color: #ddd;margin-right: 0.3rem;line-height: 1rem"
        >&#xe61c;</div>
      </div>
      <!--点击显示的图片-->
        <div v-show="block" style="width:90%; margin:auto; padding:0.3rem;" v-for="item in bh" :key="item.i">
          <img :src="item.image"  style="width:100%;" alt="">
          <div style="font-size:0.5rem; margin:0.2rem 0; text-align:left;">图片链接:{{item.url}}</div>
        </div>
      <!--回拨页面广告-->
      <div class="middle" @click="spread">
        <div class="font">回拨页面广告</div>
        <div
        v-if="hu"
          class="iconfont"
          style="font-size: 0.8rem; color: #ddd;margin-right: 0.3rem;line-height: 1rem"
        >&#xe76a;</div>
        <div
        v-else
          class="iconfont"
          style="font-size: 0.8rem; color: #ddd;margin-right: 0.3rem;line-height: 1rem"
        >&#xe61c;</div>
      </div>
      <!--点击显示的图片-->
        <div  v-show="hidden" style="width:90%; margin:auto; padding:0.3rem;" v-for="item in hu" :key="item.i">
         <img :src="item.image"  style="width:100%;" alt="">
          <div style="font-size:0.5rem; margin:0.2rem 0; text-align:left;">图片链接:{{item.url}}</div>
        </div>
    </div>
    <!--添加广告下的页面-->
    <div v-show="isShow">
      <!--添加拨号页面广告-->
      <div class="middle">
        <div class="font" style="font-weight:100;">
          <router-link :to="{path:'/add',query:{type:1}}" style=" color: #666;">拨号页面广告</router-link>
        </div>
      </div>
      <!--添加回拨页面广告-->
      <div class="middle">
        <div class="font" style="font-weight:100;">
          <router-link :to="{path:'/add',query:{type:2}}" style=" color: #666;">回拨页面广告</router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
//引入第二个header组件
import Header2 from "../components/Header2.vue";
export default {
  data() {
    return {
      show: true,
      isShow: false,
      num: 1,
      unflod:true,
      hu:true,
      bh:[],
      hu:[],
      block:true,
      hidden:true,
    };
  },
  methods: {
    // 拨号页面点击事件
    tone(){
        this.unflod=!this.unflod;
          this.axios({
        url: "/api/shop/get_banner",
        method: "post",
        headers: {
          Accept: "application/json",
          token: window.localStorage.getItem("token")
        },
        data: {
        }
      })
        .then(res => {
          console.log(res.data);
          this.bh=res.data.data.bh;
          if(this.unflod==true){
            this.block=false;
          }else{
            this.block=true;
          }
        })
        .catch(e => {
          console.log(e);
        });
    },
    // 回拨页面点击事件
    spread(){
       this.hu=!this.hu;
       this.axios({
        url: "/api/shop/get_banner",
        method: "post",
        headers: {
          Accept: "application/json",
          token: window.localStorage.getItem("token")
        },
        data: {
        }
      })
        .then(res => {
          console.log(res.data.data);
          this.hu=res.data.data.hu;
           if(this.hu==true){
            this.hidden=false;
          }else{
            this.hidden=true;
          }
        })
        .catch(e => {
          console.log(e);
        });
    },
    display() {
      this.show = true;
      this.isShow = false;
      this.num = 1;
    },
    //添加的点击事件
    add() {
      this.show = false;
      this.isShow = true;
      this.num = 2;
    }
  },
  created() {
     var token = window.localStorage.getItem("token");
  },
  //注册子组件
  components: {
    "my-header2": Header2
  }
};
</script>
<style lang="scss" scoped>
.top {
  margin-top: 0.2rem;
  font-size: 0.5rem;
  display: flex;
  justify-content: space-around;
  border-bottom: 0.3rem solid #efefef;
}
// 广告列表的文字部分
.top > div {
  margin: 0.2rem 0;
  width: 30%;
  color: #626262;
  padding: 0.3rem 0.2rem;
}
.active {
  border-bottom: 1px solid #cf9369;
}
// .top>div:hover{
//      border-bottom:1px solid #cf9369;
// }
.middle {
  padding: 0.3rem 0;
  border-bottom: 0.3rem solid #efefef;
  height: 1rem;
  display: flex;
  justify-content: space-around;
}
.middle .font {
  font-size: 0.6rem;
  font-weight: 600;
  color: #666;
  margin: auto;
  margin-left: 3.2rem;
}
</style>
