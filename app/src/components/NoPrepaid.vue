<template>
  <div class="no">
    <div class="card">
      <p>卡号/卡密</p>
      <p>套餐</p>
      <p>有效期</p>
    </div>
    <!--卡号 话费 时间 状态的内容部分-->
    <div class="content" v-for="item in list1" :key="item.i">
      <div class="number">
        <div>
          <span>卡号:</span>
          <span>{{item.number}}</span>
        </div>
        <div class="km">
          <div>
            <span>卡密:</span>
            <span>{{item.pass}}</span>
          </div>
          <div class="cz">
            <button>充值</button>
          </div>
        </div>
      </div>
      <div style="margin-top:0.4rem;">{{item.price}}元话费</div>
      <div style="margin-top:0.2rem;">
        <p>{{item.end_time}}</p>
        <!-- <p>23:59:59</p> -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props:["list1"],
  data() {
    return {};
  },
  methods:{
    child() {
            console.log("child");
        this.$emit('no');
  },
},
created(){
  var token = window.localStorage.getItem("token");
  //this.child();
},
}
</script>
<style lang="scss" scoped>
// 卡密 套餐 有效期 状态
.card {
  margin: 0.3rem 0;
  padding: 0.3rem 0;
  font-size: 0.45rem;
  display: flex;
  justify-content: space-around;
  background: #efefef;
}
// 卡号 话费 时间 状态的内容部分
.content {
  display: flex;
  font-size: 0.4rem;
  justify-content: space-around;
   padding:  0.2rem;
}
.content > div {
  width: 25%;
}
.content .number {
  text-align: left;
  width: 36%;
}
//卡密部分
.content .km {
  display: flex;
  justify-content: space-between;
}
//conten下的button
.content .cz button {
  background: #ff8936;
  color: #fff;
  font-size: 0.1rem;
  padding: 0.1rem 0.2rem;
  border-radius: 0.1rem;
}
</style>
