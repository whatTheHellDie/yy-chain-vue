<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-order">
              <ul class="order-number">
                <li>
                  <div class="number">{{!totalNumberUsdtAmount ? '0.0' : totalNumberUsdtAmount}}</div>
                  <div class="content">当前可用USDT余额（枚）</div>
                </li>
                <li>
                  <div class="number">{{!totalNumberSendYYIAmount ? '0.0' : totalNumberSendYYIAmount}}</div>
                  <div class="content">配送易用积分</div>
                </li>
                <li>
                  <div class="number">{{!totalNumberYycAmount ? '0.0' : totalNumberYycAmount}}</div>
                  <div class="content">当前可用YYC余额（枚）</div>
                </li>
              </ul>
            </div>
            <el-form :model="form" :rules="rules" ref="form" label-width="100px" class="demo-ruleForm"  style="padding:50px 0 0 50px">
              <el-form-item label="转账类型：" prop="accountType">
                <el-select v-model="form.accountType" style="width: 250px;">
                  <el-option label="USDT" value="1"></el-option>
                  <el-option label="YYC" value="2"></el-option>
                  <el-option label="配送易用积分" value="3"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="会员编号：" prop="usreNumber">
                <el-input v-if="form.userName" v-model="form.userName" disabled="disabled"></el-input>
                <el-input v-else v-model="form.userNumber" @blur="checkUserNumber"></el-input>
                <el-input type="hidden" v-model="form.userId"></el-input>
              </el-form-item>
              <el-form-item label="转账数额：" prop="withdrawalAmount">
                <el-input v-model="form.withdrawalAmount" @blur="calculationFee()"></el-input>
              </el-form-item>
              <el-form-item label="手续费：" prop="handingFee">
                <el-input v-model="form.handingFee" disabled="disabled"></el-input>
              </el-form-item>
              <el-form-item label="支付密码：" prop="payPassword">
                <el-input type="password" v-model="form.payPassword"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('form')">提交</el-button>
                <el-button @click="resetForm('form')">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
.el-input--medium{width: 250px !important;}
.elbut{
    background-color: white;
    color: #ed9d0e;
    border-color: #ed9d0e;
    margin-left: 10px;
  }
  .order-number .number {font-size: 29px;}
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  import { throws } from 'assert';
  export default {
    data () {
      let checkAmount = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('请输入转账数额'))
        } else {
          const reg = /\D/g
          if (reg.test(value)) {
            return callback(new Error('请输入数字'))
          } else if (value < 1) {
            return callback(new Error('转账数额不能低于1个'))
          } else {
            callback()
          }
        }
      }
      return {
        totalNumberUsdtAmount: '0.000000',
        totalNumberYycAmount: '0.000000',
        totalNumberSendYYIAmount: '0.000000',
        form: {
          accountType: '',
          handingFee: '',
          withdrawalAmount: '',
          payPassword: '',
          userNumber: '',
          userName: '',
          userId: ''
        },
        handingFeeRatio: '',
        walletList: [],
        rules: {
          accountType: [
            { required: true, message: '请选择转账类型', trigger: 'change' }
          ],
          withdrawalAmount: [
            { required: true, validator: checkAmount, trigger: 'blur' }
          ],
          payPassword: [
            { required: true, message: '请填写支付密码', trigger: 'blur' }
          ]
        }
      }
    },
    components: {
      MainBody
    },
    created () {
      this.loadList()
    },
    methods: {
      loadList () {
        this.$http({
          url: this.$http.adornUrl('/transfer/au/current/info'),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.totalNumberYycAmount = data.data.yyc
            this.totalNumberUsdtAmount = data.data.usdt
            this.totalNumberSendYYIAmount = data.data.sendYyi
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      calculationFee () {
        this.form.handingFee = (this.form.withdrawalAmount * this.handingFeeRatio).toFixed(2);
      },
      submitForm (formName) {
        if (this.form.accountType == 1 &&  parseFloat(this.form.withdrawalAmount + this.form.handingFee) > parseFloat(this.totalNumberUsdtAmount)) {
          return this.$message.error('USDT账户余额不足')
        } else if (this.form.accountType == 2 && parseFloat(this.form.withdrawalAmount + this.form.handingFee) > parseFloat(this.totalNumberYycAmount)) {
          return this.$message.error('YYC账户余额不足')
        } else if (this.form.accountType == 3 && parseFloat(this.form.withdrawalAmount + this.form.handingFee) > parseFloat(this.totalNumberSendYYIAmount)) {
          return this.$message.error('配送易用积分余额不足')
        }
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/transfer/au/insert'),
              method: 'post',
              data: this.$http.adornData({
                accountType: this.form.accountType,
                withdrawalAmount: this.form.withdrawalAmount,
                payPassword: this.form.payPassword,
                userNumber: this.form.userNumber,
                userName: this.form.userName,
                userId: this.form.userId
              })
            }).then(({data}) => {
              if (data && data.code === '0000') {
                this.$message({
                  message: '提交成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.loadList()
                    this.form = {}
                  }
                })
                this.$router.push({name: 'withdrawalOrder'})
              }else if (data.code === '01108') {
                this.$confirm('您还没有设置支付密码', '提示', {
                  confirmButtonText: '去设置',
                  cancelButtonText: '取消',
                  type: 'warning'
                }).then(() => {
                  this.$router.push({
                    name: 'setPaymentPassword'
                  })
                });
              } else {
                this.$message({
                  message: data.msg,
                  type: 'error'
                })
              }
            })
          } else {
            console.log('error submit!!')
            return false
          }
        })
      },
      checkUserNumber (userNumber) {
        this.$http({
          url: this.$http.adornUrl('/transfer/au/wallet/list'),
          method: 'get',
          params: this.$http.adornParams({'userNumber': userNumber})
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.form.userName = data.data.userName
            this.form.userId = data.data.userId
            this.handingFeeRatio = data.data.handingFee
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      resetForm (form) {
        this.$refs[form].resetFields()
      }
    },
    mounted () {}
  }
</script>
