<template>
  <div class="app-home">
    <header>
      <p @click="balance">余额查询</p>
      <!--输入电话好后出现的input-->
      <input
        v-if="isShow"
        type="text"
        style="background:#bc141e; color:#fff; width:40%;font-size:0.6rem;"
        v-model="phone"
      />
      <p @click="login" v-else>商家登录</p>

      <p @click="recharge">卡密充值</p>
    </header>
    <div class="img">
      <div class="swiper-container" style="height:5.8rem; margin:auto;">
        <div class="swiper-wrapper" style="color:#fff; width:100%;">
          <div class="swiper-slide" v-for="item in images" :key="item.i">
            <img :src="item.img" width="100%;" />
          </div>
        </div>
      </div>
    </div>
    <!--按钮部分-->
    <div class="button">
      <div class="math" v-for="item in list" :key="item.i" @click="button(item.num)">
        <p class="each">{{item.num}}</p>
        <p class="en">{{item.leter}}</p>
      </div>
      <div class="math">
        <p class="green">粘贴</p>
        <p class="en"></p>
      </div>
      <div class="math" @click="button(0)">
        <p class="each">0</p>
        <p class="en">+</p>
      </div>
      <div class="math">
        <p
          class="iconfont"
          style="margin-top:0.3rem; color:#16c579; font-size:0.9rem;"
          @click="del"
        >&#xe602;</p>
      </div>
    </div>
    <!--phone按钮-->
    <div style="padding:0.3rem 0;">
      <span class="iconfont" style="color:#16c579; font-size:1rem;" @click="dial">&#xe675;</span>
    </div>

  </div>
</template>
<script>
//引入第一个header组件
import header from "../components/header";
import api from "../../public/js/api.min";
import apilink from "../../public/js/apilink.min";
import Swiper from "swiper";
import "swiper/dist/css/swiper.css";
export default {
  data() {
    return {
      isShow: false,
      phone: "",
      list: [
        { num: 1, leter: "   " },
        { num: 2, leter: "ABC" },
        { num: 3, leter: "DEF" },
        { num: 4, leter: "GHI" },
        { num: 5, leter: "JKL" },
        { num: 6, leter: "MNO" },
        { num: 7, leter: "PQRS" },
        { num: 8, leter: "TUV" },
        { num: 9, leter: "WXYZ" }
      ],
      userInfo: "",
      mobile: "",
      images: []
    };
  },
  created() {
    this.getHome();
    var that = this;
    setTimeout(() => {
      that.getUserInfo();
    }, 300);
    var mobile = window.localStorage.getItem("mobile");
    var token = window.localStorage.getItem("token");
    var test = window.location.search; //获得？后面的参数
    
  },
  mounted() {
    this.get();
    var mySwiper = new Swiper(".swiper-container", {
      observer: true,
      observeParents: true,
      observeParents: false,
      loop: true,
      autoplay: {
        delay: 2000 //2秒切换一次
      },
      
    });
  },
  methods: {
    getUserInfo: function() {
      userApi.getUserInfoFun(this.setUserInfo);
    },
    setUserInfo: function(ret) {
      this.userInfo = ret.value.userInfo;
      this.mobile = ret.value.userInfo.data.mobile;
      window.localStorage.setItem("mobile", ret.value.userInfo.data.mobile);
    },

    button(num) {
      
      this.isShow = true;
      
      // console.log(e.target.innerHTML);
      if (this.phone.length < 11) {
        this.phone += num;
      }
    },
    dial() {
      this.getUserInfo();
      var p = this.phone;
      var mobile = this.mobile;
      console.log(mobile);
      var ureg = /^((13[0-9])|(14[5|7])|(15([0-3]|[5-9]))|(18[0,5-9]))\d{8}$|^((13[0-9])|(14[5|7])|(15([0-3]|[5-9]))|(18[0,5-9]))\d{8}$|^0\d{2,3}-?\d{7,8}$/;
      if (!ureg.test(p)) {
        // console.log("手机号格式不正确");
        this.$toast.center("手机号格式不正确");
        return;
      }

      this.axios({
        url: "/api/mobile/mobile",
        method: "post",
        data: {
          mynumber: mobile, //主叫号码
          callnumber: p //被叫号码
        }
      })
        .then(res => {
          console.log(res.data.data);
          this.$toast(res.data.msg);
          var img = res.data.data.img;
          var phone = res.data.data.mobile;
          this.$router.push({
            path: "/online",
            query: { phone: phone, img: img }
          });
        })
        .catch(e => {
          console.log(e);
        });
    },
    get() {
      var mobile = window.localStorage.getItem("mobile");
      this.axios({
        url: "/api/mobile/get_members_by_mobile",
        method: "post",
        data: {
          mobile: mobile
        }
      })
        .then(res => {
          console.log(res.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    // 删除按妞
    del() {
      this.phone = this.phone.substr(0, this.phone.length - 1);
    },
    login() {
      this.$router.push("/login");
    },
    balance() {
      this.$router.push("/balance");
    },
    recharge() {
      this.$router.push("/recharge");
    },
    //轮播图
    getHome() {
      var mobile = window.localStorage.getItem("mobile");
      this.axios({
        url: "/api/mobile/get_home_banner",
        method: "post",
        data: {
          mobile: mobile,
          type: 1
        }
      })
        .then(res => {
          console.log(res.data);
          this.images = res.data.data;
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  //注册子组件
  components: {
    "my-header1": header
  }
};
</script>
<style lang="scss" scoped>
.app-home {
  height: 100%;
}
.app-home .img img {
  width: 100%;
  
}
.button {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.button .math {
  width: 33%;
  height: 1.5rem;
  border-right: 1px solid #ddd;
  border-bottom: 1px solid #ddd;
}

.button .math .each {
  color: #333;
  font-size: 0.5rem;
  margin-top: 0.5rem;
}
/*图标和粘贴的字体颜色*/
.button .math .green {
  color: #16c579;
  margin-top: 20px;
  font-size: 0.5rem;
}
.button .math .en {
  font-size: 0.45rem;
  color: #ddd;
}
// 头部
header {
  width: 100%;
  height: 1.5rem;
  background: #bc141e;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  justify-content: space-around;
  line-height: 1.5rem;
  color: #fff;
  font-size: 0.45rem;
}
</style>


