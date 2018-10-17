<template>
  <div>
   <my-header></my-header>
    <el-carousel height="500px">
      <el-carousel-item v-for="item in carousel" :key="item.id">
        <img :src="item.url" class="carousel-img" />
      </el-carousel-item>
    </el-carousel>
    <div class="bdc">
      <div class="wrap">
        <div class="scroll_bg">
          <icon-svg name="laba" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>公告：
          <ul class="scroll_content" :style="{ top }">
            <li v-for="(item,index) in invit_info.text_roll">{{item}}</li>
          </ul>
          <span class="more cursor" @click="$router.push({ name: 'more' })">更多 ></span>
        </div>
      </div>
    </div>
    <div class="sell-list">
      <dl class="clearfix">
        <dd>
          <img src="/static/img/x1.png" />
          <div class="right">
            <div class="title">240,000万个</div>
            <div class="content">计划配送风水积分</div>
          </div>
        </dd>
        <dd>
          <img src="/static/img/x2.png" />
          <div class="right">
            <div class="title">135,000万个</div>
            <div class="content">计划奖励风水积分</div>
          </div>
        </dd>
        <dd class="dd3">
          <img src="/static/img/x3.png" />
          <div class="right right3">
            <div class="title">200</div>
            <div class="content">已入股人数</div>
          </div>
        </dd>
      </dl>
    </div>
    <div class="grey-bg">
      <div class="wrap">
        <img src="/static/img/jieduan1.png" alt="" class="progress-title" />
        <div class="progress-one clearfix">
          <div class="hang clearfix">
            <div class="status pull-left">进行中</div>
            <div class="qi pull-right">第 <span class="default">10</span> / 100 期</div>
          </div>
          <div class="left">
            <div role="progressbar" aria-valuenow="10" aria-valuemin="0" aria-valuemax="100" class="el-progress m-progress el-progress--circle">
              <div class="el-progress-circle" style="height: 220px; width: 220px;">
                <svg viewBox="0 0 100 100">
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke="#e5e9f2" stroke-width="4.5" fill="none" class="el-progress-circle__track"></path>
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke-linecap="round" stroke="#ed9e0d" stroke-width="4.5" fill="none" class="el-progress-circle__path" style="stroke-dasharray: 300.022px, 300.022px; stroke-dashoffset: 270.02px; transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease;"></path>
                </svg>
              </div>
              <div class="el-progress__text">10<span class="size">%</span>
                <div class="pei">已分配</div>
              </div>
            </div>
            <div class="have">本期剩余：<span>20</span>万个</div>
          </div>
          <div class="right">
            <dl>
              <dt>入股分配易用积分，并赠送100%易用积分</dt>
              <dd>
                <icon-svg name="target" class="site-sidebar__menu-icon"></icon-svg>目标配送：30,000万个</dd>
              <dd>
                <icon-svg name="qipei" class="site-sidebar__menu-icon"></icon-svg>起配积分：1万个</dd>
              <dd>
                <icon-svg name="present" class="site-sidebar__menu-icon"></icon-svg>赠送积分：赠送100%激励易用积分（1%/天，共奖励100天）</dd>
            </dl>
            <div class="time-price">
              <div class="title">当前价格</div>
              <div class="content"><span class="span">CNY：3000元/万个</span><span class="span">USDT约：2.125221枚/万个</span>
                <el-button class="gu-btn pull-right" @click="$router.push({name:'buyPackage'})">我要入股</el-button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="wrap">
        <img src="/static/img/jieduan2.png" alt="" class="progress-title" />
        <div class="progress-one progress-two clearfix">
          <div class="hang clearfix">
            <div class="status status1 pull-left">即将开始</div>
            <div class="qi pull-right">第 <span class="default">0</span> / 100 期</div>
          </div>
          <div class="left">
            <div role="progressbar" aria-valuenow="10" aria-valuemin="0" aria-valuemax="100" class="el-progress m-progress el-progress--circle">
              <div class="el-progress-circle" style="height: 220px; width: 220px;">
                <svg viewBox="0 0 100 100">
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke="#e5e9f2" stroke-width="4.5" fill="none" class="el-progress-circle__track"></path>
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke-linecap="round" stroke="#ed9e0d" stroke-width="4.5" fill="none" class="el-progress-circle__path" style="stroke-dasharray: 300.022px, 300.022px; stroke-dashoffset: 270.02px; transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease;"></path>
                </svg>
              </div>
              <div class="el-progress__text">0<span class="size">%</span>
                <div class="pei">已分配</div>
              </div>
            </div>
            <div class="have">本期剩余：<span>20</span>万个</div>
          </div>
          <div class="right">
            <dl>
              <dt>入股分配易用积分，并赠送30%易用积分</dt>
              <dd>
                <icon-svg name="target" class="site-sidebar__menu-icon"></icon-svg>目标配送：210,000万个</dd>
              <dd>
                <icon-svg name="qipei" class="site-sidebar__menu-icon"></icon-svg>起配积分：1万个</dd>
              <dd>
                <icon-svg name="present" class="site-sidebar__menu-icon"></icon-svg>赠送积分：赠送30%易用积分（3%/周，共奖励10周）</dd>
            </dl>
            <div class="time-price">
              <div class="title">当前价格</div>
              <div class="content"><span class="span">CNY：3000元/万个</span><span class="span">USDT约：2.125221枚/万个</span>
                <el-button class="gu-btn no-use pull-right">即将开始</el-button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <my-footer></my-footer>
  </div>
</template>
<style lang="scss" scoped>

</style>

<script>
  //import { getUUID } from '@/utils'
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data() {
      return {
        carousel: [{
          url: '/static/img/banner.png'
        }],
        invit_info: {
          client_money_name: '阅读币',
          text_roll: ['好消息好消息！风水链上线了！', '王五成功邀请小二，获1460阅读基金', '是游戏，获基金', 'XX']
        },
        activeIndex: 0,

        stage1: null,   //第一阶段私募
        stage2: null    //第二阶段私募
      }
    },
    components:{
      MyHeader,
      MyFooter
    },
    computed: {
      top() {
        return -this.activeIndex * 43 + 'px';
      }
    },
    mounted() {
      let _this = this;
      setInterval(function() {
        if(_this.activeIndex < _this.invit_info.text_roll.length - 1) {
          _this.activeIndex += 1;
        } else {
          _this.activeIndex = 0;
        }
      }, 3000);
    },
    created() {
         this.getInfo()
    },
    methods: {
      getInfo(){
        let _this = this;
        this.$http({
          url: this.$http.adornUrl('/stageInfo/getAll'),
          method: 'get',
          //params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === '0000') {
            
            console.log('data:'+ JSON.stringify(data))

            if (!data.data.stage1){
              _this.stage1 = data.data.stage1;
            }
            if (!data.data.stage2){
              _this.stage2 = data.data.stage2;
            }

          }
        })
      }
    }
  }
</script>