<template>
  <header>
    <div class="index-top">
      <div class="wrap">
        <icon-svg name="phone" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
        800-820-0000
        <div class="login-box clearfix">
          <span class="cursor" @click="$router.push({ name: 'login' })">登录</span>|<span class="cursor" @click="$router.push({ name: 'home' })">注册</span>
        </div>
      </div>
    </div>
    <div class="wrap">
      <div class="navbar clearfix">
        <img src="/static/img/logo.png" alt="logo" @click="$router.push({name:'index'})" class="logo" />
        <dl class="clearfix">
          <dd class="cursor active" @click="$router.push({name:'index'})">首页</dd>
          <dd class="cursor" style="padding-left: 25px;">我的账户</dd>
        </dl>
      </div>
    </div>

  </header>
</template>

<script>
</script>

<style>

</style>