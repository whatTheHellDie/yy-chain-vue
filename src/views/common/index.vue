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
            <!-- <div class="title">240,000万个</div> -->
            <div class="title">{{ sendYyiIssueAmounts }}万个</div>
            <div class="content">计划配送易用积分</div>
          </div>
        </dd>
        <dd>
          <img src="/static/img/x2.png" />
          <div class="right">
            <div class="title">{{ incentiveYyiIssueAmounts }}万个</div>
            <div class="content">计划奖励易用积分</div>
          </div>
        </dd>
        <dd class="dd3">
          <img src="/static/img/x3.png" />
          <div class="right right3">
            <div class="title">{{yyiUserCount}}</div>
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
            <!-- <div class="status pull-left">进行中</div> -->
            <div class="status pull-left" :class="{status1:statusOne}">{{ statusStr1 }}</div>
            <div class="qi pull-right">第 <span class="default">{{roundCurrent1}}</span> / {{roundAmount1}} 期</div>
          </div>
          <div class="left">
            <div role="progressbar" aria-valuenow="10" aria-valuemin="0" aria-valuemax="100" class="el-progress m-progress el-progress--circle">
              <div class="el-progress-circle" style="height: 220px; width: 220px;">
                <svg viewBox="0 0 100 100">
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke="#e5e9f2" stroke-width="4.5" fill="none" class="el-progress-circle__track"></path>
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke-linecap="round" stroke="#ed9e0d" stroke-width="4.5" fill="none" class="el-progress-circle__path" 
                    :style="{'stroke-dasharray': w1}"  style=" stroke-dashoffset: 270.02px; transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease;"></path>
                </svg>
              </div> 
              <div class="el-progress__text">{{allocationRatio1}}<span class="size">%</span>
                <div class="pei">已分配</div>
              </div>
            </div>
            <div class="have">本期剩余：<span>{{ roundSendYyiBalance1 }}</span>万个</div>
          </div>
          <div class="right">
            <dl>
              <dt>入股分配易用积分，并赠送{{giveRatio1}}%易用积分</dt>
              <dd>
                <icon-svg name="target" class="site-sidebar__menu-icon"></icon-svg>目标配送：{{ sendYyiIssueAmount1 }}万个</dd>
              <dd>
                <icon-svg name="qipei" class="site-sidebar__menu-icon"></icon-svg>起配积分：{{ startSendYyi1 }}万个</dd>
              <dd>
                <icon-svg name="present" class="site-sidebar__menu-icon"></icon-svg>赠送积分：赠送{{giveRatio1}}%激励易用积分（{{incentiveYyiRateRate1}}%/{{incentiveYyiUnit1}}，共奖励{{incentiveYyiAmount1}}{{incentiveYyiUnit1}}）</dd>
            </dl>
            <div class="time-price">
              <div class="title">当前价格</div>
              <div class="content"><span class="span">CNY：{{price1}}元/万个</span><span class="span">USDT约：{{usdtPrice}}枚/万个</span>
                <el-button class="gu-btn pull-right" @click="$router.push({name:'buyPackage'})" :disabled="disBtn1">{{textBtn1}}</el-button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="wrap">
        <img src="/static/img/jieduan2.png" alt="" class="progress-title" />
        <div class="progress-one progress-two clearfix">
          <div class="hang clearfix">
            <!-- <div class="status status1 pull-left">{{ statusStr2 }}</div> --> 
            <div class="status pull-left" :class="{status1:statusTow}">{{ statusStr2 }}</div>
            <div class="qi pull-right">第 <span class="default">{{roundCurrent2}}</span> / {{roundAmount2}} 期</div>
          </div>
          <div class="left">
            <div role="progressbar" aria-valuenow="10" aria-valuemin="0" aria-valuemax="100" class="el-progress m-progress el-progress--circle">
              <div class="el-progress-circle" style="height: 220px; width: 220px;">
                <svg viewBox="0 0 100 100">
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke="#e5e9f2" stroke-width="4.5" fill="none" class="el-progress-circle__track"></path>
                  <path d="M 50 50 m 0 -47 a 47 47 0 1 1 0 94 a 47 47 0 1 1 0 -94" stroke-linecap="round" stroke="#ed9e0d" stroke-width="4.5" fill="none" class="el-progress-circle__path" 
                    :style="{'stroke-dasharray': w2}" style="stroke-dashoffset: 270.02px; transition: stroke-dashoffset 0.6s ease 0s, stroke 0.6s ease;"></path>
                <!-- style="stroke-dasharray: 300.022px, 300.022px; -->
                </svg>
              </div>
              <div class="el-progress__text">{{ allocationRatio2 }}<span class="size">%</span>
                <div class="pei">已分配</div>
              </div>
            </div>
            <div class="have">本期剩余：<span>{{ roundSendYyiBalance2 }}</span>万个</div>
          </div>
          <div class="right">
            <dl>
              <dt>入股分配易用积分，并赠送{{giveRatio2}}%易用积分</dt>
              <dd>
                <icon-svg name="target" class="site-sidebar__menu-icon"></icon-svg>目标配送：{{ sendYyiIssueAmount2 }}万个</dd>
              <dd>
                <icon-svg name="qipei" class="site-sidebar__menu-icon"></icon-svg>起配积分：{{ startSendYyi2 }}万个</dd>
              <dd>
                <icon-svg name="present" class="site-sidebar__menu-icon"></icon-svg>赠送积分：赠送{{giveRatio2}}%易用积分（{{incentiveYyiRate2}}%/{{incentiveYyiUnit2}}，共奖励{{incentiveYyiAmount2}}{{incentiveYyiUnit2}}）</dd>
            </dl>
            <div class="time-price">
              <div class="title">当前价格</div>
              <div class="content"><span class="span">CNY：{{price2}}元/万个</span><span class="span">USDT约：{{usdtPrice}}枚/万个</span>
                <!-- <el-button class="gu-btn no-use pull-right">即将开始</el-button> -->
                <el-button class="gu-btn pull-right" @click="$router.push({name:'buyPackage'})" :disabled="disBtn2">{{textBtn2}}</el-button>
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
  // import { getUUID } from '@/utils'
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
          text_roll: ['好消息好消息！易用链上线了！', '王五成功邀请小二，获1460阅读基金', '是游戏，获基金', 'XX']
        },
        activeIndex: 0,

        stage1: null,   //第一阶段私募
        stage2: null,    //第二阶段私募
        sendYyiIssueAmounts: 0, //计划配送易用积分
        incentiveYyiIssueAmounts: 0, //计划奖励易用积分
        yyiUserCount: 0, //入股总人数

        statusOne: true,        //第一阶段根据状态是否启用样式
        statusStr1: '即将开始',  //第一阶段状态
        roundCurrent1: 0, //第一阶段当前期数
        roundAmount1: 0, //第一阶段总期数
        roundSendYyiBalance1: 0,//第一阶段本期剩余
        allocationRatio1: 0,  //第一阶段分配率
        sendYyiIssueAmount1: 0,  //第一阶段目标配送
        startSendYyi1: 0,//第一阶段起配积分
        giveRatio1: 0, //第一阶段赠送比例
        incentiveYyiRate1: 0, //第一阶段赠送率 
        price1: 0, //第一阶段价格
        incentiveYyiAmount1: 0, //第一阶段共奖励？天或周
        incentiveYyiUnit1: '天', //第一阶段激励易用积分赠送单位1=每天，2=每周
        disBtn1: true,  //第一阶段入股操作按钮是否可用
        textBtn1: '即将开始', //第一阶段入股操作按钮显示文本
        
        statusTow: true,        //第二阶段根据状态是否启用样式
        statusStr2: '即将开始',  //第二阶段状态
        roundCurrent2: 0, //第二阶段当前期数
        roundAmount2: 100, //第二阶段总期数
        roundSendYyiBalance2: 20,//第二阶段本期剩余
        allocationRatio2: 0,  //第二阶段分配率
        sendYyiIssueAmount2: 210000,  //第二阶段目标配送
        startSendYyi2: 1,//第二阶段起配积分
        giveRatio2: 30, //第二阶段赠送比例
        incentiveYyiRate2: 3, //第二阶段赠送率 
        price2: 3000, //第二阶段价格
        incentiveYyiAmount2: 10, //第二阶段共奖励？天或周
        incentiveYyiUnit2: '周', //第二阶段激励易用积分赠送单位1=每天，2=每周
        disBtn2: true,  //第二阶段入股操作按钮是否可用
        textBtn2: '即将开始', //第二阶段入股操作按钮显示文本

        usdtPrice: 1,  //美元价格
 
        w1: '270px,300.022px',  //默认样式 第一阶段已分配比例的画图样式【0% = '270px,300.022px'，100% = '564px,300.022px'，564 = (100 * 2.94) + 270】
        w2: '270px,300.022px' , //默认样式 第二阶段已分配比例的画图样式【0% = '270px,300.022px'，100% = '564px,300.022px'，564 = (100 * 2.94) + 270】
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
          // params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === '0000') {

            console.log('data:'+ JSON.stringify(data))
            if (data.data.sendYyiIssueAmounts){
              _this.sendYyiIssueAmounts = this.resetVal(data.data.sendYyiIssueAmounts); 
            }
            if (data.data.incentiveYyiIssueAmounts){
              _this.incentiveYyiIssueAmounts = this.resetVal(data.data.incentiveYyiIssueAmounts);  
            }
            if (data.data.yyiUserCount){
              _this.yyiUserCount = data.data.yyiUserCount;
            }
            //第一阶段
            if (data.data.stage1){
              _this.stage1 = data.data.stage1; 
              _this.statusOne = _this.stage1.status == 0 ? true : _this.stage1.status == 1 ? false : _this.stage1.status == 2 ? true : true;
              _this.statusStr1 = _this.statusZh(_this.stage1.status); 
              _this.roundCurrent1 = _this.stage1.roundCurrent;
              _this.roundAmount1 = _this.stage1.roundAmount;
              _this.roundSendYyiBalance1 = this.resetVal(_this.stage1.roundSendYyiBalance);
              _this.sendYyiIssueAmount1 = this.resetVal(_this.stage1.sendYyiIssueAmount);  
              _this.allocationRatio1 = _this.stage1.allocationRatio; 
              _this.startSendYyi1 = _this.stage1.startSendYyi;  
              _this.giveRatio1 = _this.stage1.giveRatio;
              _this.incentiveYyiRate1 = _this.stage1.incentiveYyiRate;
              _this.incentiveYyiAmount1 = _this.stage1.incentiveYyiAmount;
              _this.incentiveYyiUnit1 = _this.returnUnit(_this.stage1.incentiveYyiUnit);
              _this.price1 = _this.stage1.price;
              _this.usdtPrice = _this.stage1.usdtPrice;
              _this.disBtn1 = _this.stage1.status == 0 ? true : _this.stage1.status == 1 ? false : _this.stage1.status == 2 ? true : true;
              _this.textBtn1 = _this.stage1.status == 0 ? '即将开始' : _this.stage1.status == 1 ? '我要入股' : _this.stage1.status == 2 ? '已结束' : '即将开始';
              //已分配百分率样式计算（获取百分率所在的分辨率）
              let h1 = (_this.allocationRatio1 * 2.94) + 270;
              // _this.h1 = (100 * 2.94) + 270; //100%
              _this.w1 = h1 + ',300.022px';
            }
            //第二阶段
            if (data.data.stage2){
              _this.stage2 = data.data.stage2;
              _this.statusTow = _this.stage2.status == 0 ? true : _this.stage2.status == 1 ? false : _this.stage2.status == 2 ? true : true;
              _this.statusStr2 = _this.statusZh(_this.stage2.status); 
              _this.roundCurrent2 = _this.stage2.roundCurrent;
              _this.roundAmount2 = _this.stage2.roundAmount;
              _this.roundSendYyiBalance2 = this.resetVal(_this.stage2.roundSendYyiBalance);
              _this.sendYyiIssueAmount2 = this.resetVal(_this.stage2.sendYyiIssueAmount);  
              _this.allocationRatio2 = _this.stage2.allocationRatio; 
              _this.startSendYyi2 = _this.stage2.startSendYyi;  
              _this.giveRatio2 = _this.stage2.giveRatio;
              _this.incentiveYyiRate2 = _this.stage2.incentiveYyiRate;
              _this.incentiveYyiAmount2 = _this.stage2.incentiveYyiAmount;
              _this.incentiveYyiUnit2 = _this.returnUnit(_this.stage2.incentiveYyiUnit);
              _this.price2 = _this.stage2.price;
              _this.usdtPrice = _this.stage2.usdtPrice;
              _this.disBtn2 = _this.stage2.status == 0 ? true : _this.stage2.status == 1 ? false : _this.stage2.status == 2 ? true : true;
              _this.textBtn2 = _this.stage2.status == 0 ? '即将开始' : _this.stage2.status == 1 ? '我要入股' : _this.stage2.status == 2 ? '已结束' : '即将开始';
              //已分配百分率样式计算
              let h2 = (_this.allocationRatio2 * 2.94) + 270;
              _this.w2 = h2 + ',300.022px';
            }

          }
        })
      },
      returnUnit(unit){
        return unit == 1 ? '天' : unit == 2 ? '周' : '天';
      }, 
      //状态类型转换为明文显示
      statusZh(status){
        return status == 0 ? '即将开始' : status == 1 ? '进行中' : status == 2 ? '已结束' : '即将开始';
      },
      //数字格式化,保留两位小数并每三位加逗号
      resetVal (number) {
        var mNumber = parseInt(number)
        number = number.toString()
        var point = number.split('.')[1];
        console.log(point)
        mNumber = mNumber.toString()
        if (mNumber.length <= 3){
          if(!point) {
            return (mNumber == '' ? '0' : mNumber)
          }else{
            return (mNumber == '' ? '0' : mNumber) + '.' + point
          }
        }else {
          var mod = mNumber.length % 3;
          var output = (mod == 0 ? '' : (mNumber.substring(0, mod)))
          for (var i = 0; i < Math.floor(mNumber.length / 3); i++) {
            if ((mod == 0) && (i == 0)) {
              output += mNumber.substring(mod + 3 * i, mod + 3 * i + 3)
            } else {
              output += ',' + mNumber.substring(mod + 3 * i, mod + 3 * i + 3)
            }
          }
          console.log(typeof point)
          if(!point){ 
            return output
          }else{
            return output + '.' + point 
          }
        } 
      },
    }
  }
</script>
