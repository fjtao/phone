<template>
  <div class="balance">
    <header>
            <div @click="back" style="margin-left:-0.4rem;">
              <span class=" iconfont" style="font-size: 0.5rem;">&#xe620; </span>
              <span>返回</span>
            </div>
            <div @click="balance">余额查询</div>
            <div @click="billed">话单查询</div>
    </header>
    <!--上边的input框-->
    <div class="top">
      <div class="input">
        <span>账户:</span>
        <span>{{mobile}}</span>
      </div>
    </div>
    <!--bottom部分-->
    <div class="bottom">
      <div class="input">
        <span>话费余额:</span>
        <span>{{price}}</span>
      </div>
      <div style="border-bottom:1px solid #ddd; margin:0 0.5rem;"></div>
      <div class="input">
        <span>账户有效期:</span>
        
        <span>{{time}}</span>
      </div>
    </div>
    <!--温馨提示部分-->
    <div class="hint">
      <p>温馨提示:</p>
      <p>请注意您的账户有效期与话费余额。</p>
    </div>
  </div>
</template>
<script>
//引入第一个header组件
import header from "../components/header";
export default {
  data() {
    return {
      
      time:"",
      price:"",
      mobile:"",
    };
  },
  methods: {
    balan() {
      var mobile = window.localStorage.getItem("mobile");
      this.axios({
        url: "/api/mobile/get_mobile_info",
        method: "post",
        data: {
           
          mobile: mobile,
        }
      })
        .then(res => {
          console.log(res.data.data);
          this.mobile=res.data.data.mobile;
          this.time=res.data.data.e_time;
          this.price=res.data.data.ka_mi_price+"元"
        })
        .catch(e => {
          console.log(e);
        });
    },
      back(){
            this.$router.go(-1);
        },
        balance(){
            this.$router.push("/balance");
        },
        billed(){
            this.$router.push("/billed");
        }
  },
  created() {
     var mobile = window.localStorage.getItem("mobile");
    this.balan();
  },
  
};
</script>
<style lang="scss" scoped>
.balance {
  background: #f5f5f5;
  height: 20rem;
}
// 文字账户
.input span {
  display: flex;
  align-items: center;
  font-size: 0.45rem;
}
.input {
  display: flex;
  background: #ffffff;
  border: 1px solid #ffffff;
  padding: 0.3rem 0.5rem;
  justify-content: space-between;
}
.input input {
  width: 50%;
  padding: 0.2rem;
  padding-left: 0.5rem;
  font-size: 0.45rem;
}
//top部分
.top {
  margin: 1rem 0;
}
// 温馨提示部分
.hint {
  font-size: 0.42rem;
  padding: 0 0.5rem;
  margin-top: 3rem;
}
.hint p {
  color: #828282;
  text-align: left;
  margin: 0.1rem 0;
  line-height: 0.5rem;
}
 header{
        width: 100%;
        height: 1.5rem;
        background: #bc141e;
        display: flex; 
        justify-content: space-around;
        line-height: 1.5rem;
        color: #fff;
        font-size: 0.45rem;
        
    }
</style>
