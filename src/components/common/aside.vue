<template>
  <div class="left-navbar">
    <dl>
      <dt class="cursor" :class="{ active: $route.name == 'accountIndex' }" @click="$router.push({ name: 'accountIndex' })"><icon-svg :name="iconName" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>账户首页</dt>
    </dl>
    <dl>
      <dt class="cursor" :class="{ active: $route.name == 'personalInformation' }" @click="$router.push({ name: 'personalInformation' })"><icon-svg :name="iconName1" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>个人中心</dt>
    </dl>
    <dl>
      <dt><icon-svg name="order" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>订单管理</dt>
      <dd v-for="item in sideNav.order" :class="{active:$route.name == item.routeName }" @click="$router.push({ name: item.routeName })">{{item.name}}</dd>
    </dl>
    <dl>
      <dt><icon-svg name="money" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>资金管理</dt>
      <dd v-for="item in sideNav.fund" :class="{active:$route.name == item.routeName }" @click="$router.push({ name: item.routeName })">{{item.name}}</dd>
    </dl>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        num1: 1,
        sideNav: {
          account: [ //账户管理
//          {
//            name: "个人信息",
//            routeName: "personalInformation"
//          },
//          {
//            name: "实名认证",
//            routeName: "realNameAuthentication"
//          },
//          {
//            name: "设置支付密码",
//            routeName: "setPaymentPassword"
//          },
//          {
//            name: "忘记支付密码",
//            routeName: "forgetPaymentPassword"
//          },
//          {
//            name: "重置支付密码",
//            routeName: "resetPaymentPassword"
//          },
//          {
//            name: "重置登录密码",
//            routeName: "resetLoginPassword"
//          },
          ],
          order: [ //订单管理
            {
              name: "入股订单",
              routeName: "investmentShareOrder"
            },
            {
              name: "充币订单",
              routeName: "chargeCoinOrder"
            }
          ],
          fund: [ //资金管理
            {
              name: "充币",
              routeName: "chargeCoin"
            },
            {
              name: "资金明细",
              routeName: "CapitalDetails"
            }
          ],
        }
      }
    },
    computed: {
      iconName(){
        return this.$route.name == 'accountIndex'?"index1":"index"
        },
        iconName1(){
        return this.$route.name == 'personalInformation'?"person1":"person";
        }
    },
    methods: {},
    mounted() {
      console.log(this.$route.name)
    }
  }
</script>

<style>
.left-navbar dt.active {
    color: #ed9d0e;
  }
  .left-navbar dd.active {
    color: #ed9d0e;
  }
</style>