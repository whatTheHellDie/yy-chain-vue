<template>
  <header>
    <div class="index-top">
      <div class="wrap">
        <!--<icon-svg name="phone" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>-->
        <div class="login-box clearfix">
          <div v-if="loginStatus==0"><span class="span cursor" @click="$router.push({ name: 'login' })">登录</span>|<span class="span cursor" @click="$router.push({ name: 'register' })">注册</span></div>
          <div v-if="loginStatus==1">尊敬的<span class="white">{{loginUserNumber}}</span>用户，欢迎您来到易用链！
            <span class="logout cursor" @click="logout()">注销</span>
          </div>
        </div>
      </div>
    </div>
    <div class="wrap">
      <div class="navbar clearfix">
        <img src="/static/img/logo.png" alt="logo" @click="$router.push({name:'index'})" class="logo" />
        <dl class="clearfix">
          <dd class="cursor" :class="{active:myNavIndex==0}" @click="$router.push({name:'index'})">首页</dd>
          <dd class="cursor" :class="{active:myNavIndex==1}" @click="toAccount" style="padding-left: 25px;">我的账户</dd>
        </dl>
      </div>
    </div>

  </header>
</template>

<script>
  export default {
    data() {
      return {
        loginStatus: '0',
        loginUserNumber: window.sessionStorage.getItem('userNumber')
      }
    },
    props: ['navIndex'],
    computed: {
      myNavIndex() {
        if(!this.navIndex) {
          return 0
        } else {
          return this.navIndex
        }
      }
    },
    created() {
      if(this.$cookie.get('yy-chain-token') && window.sessionStorage.getItem('userNumber')) {
        this.loginStatus = 1
      }
      if(this.$route.name == 'register' || this.$route.name == 'login') {
        this.loginStatus = 2
      }
    },
    methods: {
      logout() {
        this.$http({
          url: this.$http.adornUrl('/user/logout'),
          method: 'get'
        }).then(({
          data
        }) => {
          if(data && data.code === '0000') {
            this.$cookie.delete('yy-chain-token')
            this.$message.success('注销成功')
            this.loginStatus = '0'
            this.$router.push({
              name: 'login'
            })
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      toAccount() {
        if(this.$cookie.get('yy-chain-token') && window.sessionStorage.getItem('userNumber')) {
          this.$router.push({
            name: 'accountIndex'
          })
        } else if(window.sessionStorage.getItem('userNumber')) {
          this.$confirm('您还没有登录，请登录', '提示', {
            confirmButtonText: '去登录',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$router.push({
              name: 'login'
            })
          })
        } else {
          this.$confirm('您还没有登录，请登录', '提示', {
            confirmButtonText: '去登录',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$router.push({
              name: 'login'
            })
          })
        }
      }
    }
  }
</script>

<style>

</style>
