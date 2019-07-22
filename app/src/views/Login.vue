<template>
    <div class="login">
        <my-header2>
            <div slot="title">商家登录</div>
        </my-header2>
        <div class="body">
            <!--输入框-->
            <div class="account">
                 <p>帐号</p>
                 <input type="text" placeholder="请输入帐号" v-model="name">
            </div>
            <div class="account">
                 <p>密码</p>
                 <input type="password" placeholder="请输入密码" v-model="upwd" >
            </div>
            <!--登录按钮-->
            <div class="button">
                <button @click="login">商家登录</button>
            </div>
        </div>
        
    </div>
</template>
<script>
//引入第二个header组件
import Header2 from "../components/Header2.vue";
export default {
    data(){
        return {
            name:"",
            upwd:"",
        }
    },
    methods:{
        //验证登录
        login(){
            var n=this.name;
            var p=this.upwd;
            var ureg=/^[a-z0-9_]{3,20}$/;
            if(!ureg.test(n)){
                // console.log("用户名格式不正确");
                 this.$toast.center("用户名格式不正确");
                return;
            }
            var preg=/^[a-z0-9]{3,16}$/;
            if(!preg.test(p)){
                // console.log("密码格式不正确");
                 this.$toast.center("密码格式不正确");
                return;
            }
             //发送ajax请求 axios
            //查看输入内容与数据库中内容进行对比
            
            this.axios({
                url:'/api/login/login',
                method:'post',
                data:{
                    s_name:n,
                    password:p
                }
            }).then(res=>{
                console.log(res.data)
                window.localStorage.setItem('token',res.data.data.token);
                if(res.data.code==0){
                     this.$toast.center(res.data.msg);
                    this.$router.push("/business");
                }else{
                     this.$toast.center(res.data.msg);
                }
            }).catch(e=>{
                console.log(e)
            })
        }
    },
     //注册子组件
  components:{
    "my-header2":Header2
  },
}
</script>
<style lang="scss" scoped>
::-webkit-input-placeholder{
    color:#ddd;
    font-size: 0.45rem;
}
    .account{
        display: flex;
        font-size: 0.45rem;
        margin: auto;
        border-bottom: 1px solid #ddd;
        padding: 0.2rem 0;
    }
    // 文字帐号
    .account p{
        margin-top:0.35rem;
        font-size: 0.45rem;
    }
    .account input{
        width: 80%;
        font-size: 0.45rem;
        padding: 0.2rem 0.3rem;
        align-items: center;
    }
    .body{
        margin-top: 1.5rem;
        margin-left: 0.4rem;
        margin-right: 0.4rem;
    }
    //button登录按钮
    .button{
        margin-top:1.6rem;
    }
    .button button{
        background: #ff8936;
        color: #fff;
        padding: 0.2rem 1rem;
        border-radius: 0.2rem;
        font-size: 0.45rem; 
    }
</style>
