<template>
  <div id="app">
    <header>
      <div class="login_top">
        <ul class="login_nav">
          <li><router-link to="/cart" class="login">购物车({{user.cartnum}})</router-link></li>
          <li @click="changeLoginway('login')" v-if="isLogin==0"><router-link to="/login" class="login">登录&nbsp;Login</router-link></li>
          <li @click="changeLoginway('register')" v-if="isLogin==0"><router-link to="/login" class="register">注册&nbsp;Register</router-link></li>
          <li v-if="isLogin==1"><router-link to="/usercenter">你好，<span class="nick">{{user.nick}}</span></router-link></li>
          <li @click="Logout()" v-if="isLogin==1"><router-link to="/home" class="login">注销</router-link></li>
        </ul>
      </div>
      <div class="nav">
        <ul class="left_nav">
          <li :class="{bg:show===left_nav_value}" v-for="left_nav_value in left_navs">
            <router-link :to="'/'+left_nav_value" class="nav_a">{{left_nav_value.toUpperCase()}}</router-link>
          </li>
        </ul>
        <ul class="right_nav">
          <li :class="{bg:show===right_nav_value}" v-for="right_nav_value in right_navs">
            <router-link :to="'/'+right_nav_value" class="nav_a">{{right_nav_value.toUpperCase()}}</router-link>
          </li>
        </ul>
        <h1 class="theme">
          <router-link to="/home">Ahutaoer</router-link>
        </h1>
      </div>
    </header>
    <section>
      <transition name="display">
        <router-view></router-view>
      </transition>
    </section>
    <footer>
      <ul class="bottom">
        <li v-for="item in items" class="items">
          <h1><router-link to='/home'>{{item.title}}</router-link></h1>
          <ul>
            <li v-for="content in item.contents" class="item"><router-link to='/home'>{{content}}</router-link></li>
          </ul>
        </li>
      </ul>
    </footer>
    <div v-show="showgotoTop" class="scroll" @click="toTop">
      <img :src="'../static/images/scroll.jpg'" alt="scrollTop">
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  data() {
    return {
      showgotoTop: false
    };
  },
  computed: {
    ...mapGetters({
      show: "getShow",
      items: "getFootItems",
      left_navs: "getLeft_nav",
      right_navs: "getRight_nav",
      isLogin:"getIsLogin",
      user:"getUser"
    })
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  methods: {
    Logout(){
      this.$store.commit('CHANGE_LOGIN','0');
      layer.msg('注销成功!');
      this.$store.commit('CHANGE_USER',{
        cartnum:0
      })
      setTimeout(function(){
        window.location.reload();
      },1000)
    },
    toTop() {
      var gotoTop = function() {
        var currentPosition =
          document.documentElement.scrollTop || document.body.scrollTop;
        currentPosition -= 40;
        if (currentPosition > 0) {
          window.scrollTo(0, currentPosition);
        } else {
          window.scrollTo(0, 0);
          clearInterval(timer);
          timer = null;
        }
      };
      var timer = setInterval(gotoTop, 1);
    },
    changeLoginway(type) {
      this.$store.dispatch("changeLoginway", type);
    },
    handleScroll() {
      this.showgotoTop = window.scrollY >= 50 ? true : false;
    }
  },

  created() {
    var self = this;
    var a = 1;
    window.onload = () => {
      this.$store.dispatch("change_hw", {
        h: document.documentElement.clientHeight || document.body.clientHeight,
        w: document.documentElement.clientWidth || document.body.clientWidth
      });
    };
    window.onresize = () => {
      if (self.timer) {
        clearTimeout(self.timer); //函数节流
      }
      self.timer = setTimeout(function() {
        self.$store.dispatch("change_hw", {
          h:
            document.documentElement.clientHeight || document.body.clientHeight,
          w: document.documentElement.clientWidth || document.body.clientWidth
        });
      }, 100);
    };
    window.onscroll = () => {
      var scrollTop =
        document.documentElement.scrollTop ||
        window.pageYOffset ||
        document.body.scrollTop;
      if (scrollTop > 10) {
        this.scroll = true;
      } else {
        this.scroll = false;
      }
    };
    //将vuex中的数据存入浏览器缓存中，以解决刷新后数据丢失的问题
    localStorage.getItem("storeMsg") && this.$store.replaceState(Object.assign(this.$store.state,JSON.parse(localStorage.getItem("storeMsg"))));
    window.addEventListener("beforeunload",()=>{
        localStorage.setItem("storeMsg",JSON.stringify(this.$store.state))
    })
  }
};
</script>

<style>
body,
form,
div,
span,
ul,
ol,
li,
p,
pre,
dl,
dt,
dd,
h1,
h2,
h3,
h4,
h5,
h6,
em,
a,
fieldset,
legend,
address,
label,
textarea,
select,
input,
figure,
table,
th,
td,
b,
i {
  margin: 0;
  padding: 0;
}
fieldset,
img {
  border: 0;
}
ul,
li {
  list-style: none;
}
em,
i {
  font-style: normal;
}
table {
  border-collapse: separate;
  border-spacing: 0;
}
a {
  text-decoration: none;
  color: black;
}
a:hover{
  text-decoration: none
}
a:focus,
input,
button,
select,
textarea {
  outline: none;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: 100%;
  font-variant: normal;
  font-weight: 400;
}
body {
  margin: 0px;
  width: 100%;
  font-family: "Segoe UI", "Lucida Grande", Helvetica, Arial, "Microsoft YaHei",
    FreeSans, Arimo, "Droid Sans", "wenquanyi micro hei", "Hiragino Sans GB",
    "Hiragino Sans GB W3", FontAwesome, sans-serif;
  font-size: 16px;
}
input {
  text-shadow: none;
}
#app {
  width: 100%;
}
.nick{
  color: #13f5cd;
}
.nick:hover{
  text-decoration: underline
}
header {
  height: 120px;
  min-width: 1150px;
}
.login_top {
  background: #3d444c;
  height: 30px;
}
.login_nav {
  float: right;
  margin-right: 0;
}
.login_nav li {
  display: inline-block;
  padding: 3px 3px;
}
.login_nav li a {
  color: #fff;
}
.login_nav li:nth-child(2) {
  padding-right: 6px;
  border-right: 2px solid #fff;
}
.login_nav .login {
  cursor: pointer;
}
.login_nav .register {
  cursor: pointer;
}
.login_nav .login:hover,
.login_nav .register:hover {
  text-decoration: underline;
}
.left_nav {
  float: left;
  margin-left: 15px;
}
.left_nav li,
.right_nav li {
  display: inline-block;
  text-align: center;
  height: 90px;
  line-height: 90px;
  font-size: 20px;
  font-family: verdana;
  transition: all 0.3s ease-in-out;
  cursor: pointer;
}
.left_nav li:hover {
  background: #00bc9b;
}
.right_nav {
  float: right;
  margin-right: 15px;
}
.right_nav li:hover {
  background: #00bc9b;
}
.nav_a {
  display: inline-block;
  height: 90px;
  padding: 0 85px;
}
.nav {
  width: 100%;
  overflow: hidden;
}
header h1 {
  width: 200px;
  height: 90px;
  text-align: center;
  line-height: 90px;
  font-size: 32px;
  font-family: verdana;
  cursor: pointer;
  transition: all 0.5s ease;
  margin: 0 auto;
}
header h1:hover {
  font-size: 42px;
}
footer {
  height: 72px;
  background: #3d444c;
  width: 100%;
  min-width: 1150px;
  padding-top: 8px;
  position: relative;
  color: #a9b8ca;
}
.bg {
  background: #00bc9b;
}
footer .items {
  display: inline-block;
  padding: 0 2px;
}
footer strong {
  position: absolute;
  left: 0;
  top: 27.5px;
}
footer strong a {
  text-decoration: underline;
  color: #a9b8ca;
}
footer .item a {
  color: #a9b8ca;
  font-size: 12px;
}
footer ul {
  text-align: center;
}
footer ul li {
  padding: 0 20px;
}
footer h1 {
  text-decoration: none;
}
footer h1 a {
  color: #fff;
}
footer .item:hover > a {
  text-decoration: underline;
  color: #08c;
}
.scroll {
  width: 60px;
  position: fixed;
  bottom: 80px;
  right: 50px;
}
.display-enter,
.display-leave-active {
  opacity: 0;
}
.display-enter-active,
.display-leave {
  transition: all 0.5s ease;
  -moz-transition: all 0.5s ease; /* Firefox 4 */
  -webkit-transition: all 0.5s ease; /* Safari 和 Chrome */
  -o-transition: all 0.5s ease; /* Opera */
}
</style>
