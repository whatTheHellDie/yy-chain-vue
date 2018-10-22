<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="index-i">
              <img src="/static/img/profile.png" alt="" class="tou">
              <div class="title">{{userNumber}}
                <div class="renzheng">
                  <div class="icon" :class="{active:checkPerson}" style="background-image:url(/static/img/i1.png) ;" :title="person"></div> |
                  <div class="icon" :class="{active:checkPaymentPassword}" style="background-image:url(/static/img/i2.png) ;" :title="paymentPassword"></div> |
                  <div class="icon active" style="background-image:url(/static/img/i3.png) ;" title="手机号码已设置"></div>
                </div>
              </div>
            </div>
            <div class="my-zichang hei212">
              <div class="title">账户资产</div>
              <div class="clear"></div>
              <div class="money-list clearfix">
                <div class="w33">
                  <div class="name">YYC（枚）</div>
                  <div class="content" v-html="resetVal(fund.yyc)"></div>
                </div>
                <div class="w33">
                  <div class="name">USDT（枚）</div>
                  <div class="content" v-html="resetVal(fund.usdt)"></div>
                </div>
                <div class="w33">
                  <div class="name">易用积分（个）</div>
                  <div class="content" v-html="resetVal(fund.yyi)"></div>
                  <div class="hover-content">
                    <div class="clearfix">配送易用积分<span class="pull-right">{{fund.sendYyi}}</span></div>
                    <div class="clearfix">激励易用积分<span class="pull-right">{{fund.incentiveYyi}}</span></div>
                    <div class="clearfix">未到账易用积分<span class="pull-right">{{fund.frozenYyi}}</span></div>
                  </div>
                </div>
              </div>
            </div>
            <div class="my-zichang hei212" style="border-top: 0;">
              <div class="title">直推概况</div>
              <div class="clear"></div>
              <div class="money-list clearfix">
                <div class="w33">
                  <div class="name">直推会员人数</div>
                  <div class="content" v-html="resetVal(fund.directlyUserCount)"></div>
                </div>
                <div class="w33">
                  <div class="name">累计获得USDT数额</div>
                  <div class="content" v-html="resetVal(fund.usdt)"></div>
                </div>
                <div class="w33">
                  <div class="name">累计获得奖励易用积分数额</div>
                  <div class="content" v-html="resetVal(fund.yyi)"></div>
                </div>
              </div>
            </div>
            <div class="my-zichang">
              <div class="title">团队概况</div>
              <div class="clear"></div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    团队总人数(人)
                  </div>
                  <div class="number">0</div>

                </div>
                <div class="right clearfix">
                  <div class="w33">
                    <div class="population">vip会员(人)</div>
                    <div class="s-number">0</div>
                  </div>
                  <div class="w33">
                    <div class="population">普通会员(人)</div>
                    <div class="s-number">0</div>
                  </div>
                  <div class="w33">
                    <div class="population">其他(人)</div>
                    <div class="s-number">0</div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    累计获得奖励易用积分数(个)
                  </div>
                  <div class="number" v-html="resetVal(0)"></div>
                </div>
                <div class="right clearfix">
                  <div class="w50">
                    <div class="population">今日获得(个)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                  <div class="w50">
                    <div class="population">本月获得(个)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                  累计获得USDT数额(枚)
                  </div>
                  <div class="number">0</div>

                </div>
                <div class="right clearfix">
                  <div class="w33 w50">
                    <div class="population">今日获得(枚)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                  <div class="w33 w50">
                    <div class="population">本月获得(枚)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    团队总人数(人)
                  </div>
                  <div class="number">0</div>

                </div>
                <div class="right clearfix">
                  <div class="w33 w50">
                    <div class="population">今日业绩(个)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                  <div class="w33 w50">
                    <div class="population">本月业绩(个)</div>
                    <div class="s-number" v-html="resetVal(0)"></div>
                  </div>
                </div>
              </div>
            </div>


          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
  .bold {
    .el-breadcrumb__item:last-child .el-breadcrumb__inner {
      font-weight: 800;
    }
  }
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        checkPerson: false,
        person: '身份未认证',
        checkPaymentPassword: false,
        paymentPassword: '支付密码未设置',
        activeNumber: 0,
        chooseList: [{
          name: '全部'
        }, {
          name: '待支付'
        }, {
          name: '已完成'
        }, {
          name: '交易关闭'
        }, {
          name: '交易取消'
        }],
        todayGet: '234.001233',
        userNumber: '',
        fund: {
          yyc: '',
          yyi: '',
          usdt: '',
          sendYyi: '',
          incentiveYyi: '',
          frozenYyi: '',
          directlyUserCount: ''
        }
      }
    },
    components: {
      MainBody
    },
    methods: {
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
          return (output + '.<span class="little-num">' + point + '</span>')

          }
        }
        //      return `${val.parseInt}.<span class="little-word">${dian}</span>`
      },
      personFund () {
        this.$http({
          url: this.$http.adornUrl('/fund/au/person'),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.userNumber = data.data.userNumber
            this.fund.yyc = data.data.yyc
            this.fund.yyi = data.data.yyi
            this.fund.usdt = data.data.usdt
            this.fund.sendYyi = data.data.sendYyi
            this.fund.incentiveYyi = data.data.incentiveYyi
            this.fund.frozenYyi = data.data.frozenYyi
            this.fund.directlyUserCount = data.data.directlyUserCount

            if (data.data.payPassword !== '') {
              this.checkPaymentPassword = true
              this.paymentPassword = '支付密码已设置'
            }
            if (data.data.auth === '3') {
              this.checkPerson = true
              this.person = '身份已认证'
            }
          } else {
            this.$message.error(data.msg)
          }
        }).catch(({error}) => {
          this.dataListLoading = false
          this.$message.error(error)
        })
      }
    },
    mounted: function () {
      this.personFund()
    }
  }
</script>
