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
                  <div class="number">{{!totalNumberYycAmount ? '0.0' : totalNumberYycAmount}}</div>
                  <div class="content">当前可用YYC余额（枚）</div>
                </li>
              </ul>
            </div>
            <el-form :model="form" :rules="rules" ref="form" label-width="100px" class="demo-ruleForm"  style="padding:50px 0 0 50px">
              <el-form-item label="提币类型：" prop="accountType">
                <el-select v-model="form.accountType" @change="getWalletList(form.accountType)" style="width: 250px;">
                  <el-option label="USDT" value="1"></el-option>
                  <el-option label="YYC" value="2"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="数字钱包：" prop="walletId">
                <el-select v-model="form.walletId" @change="selectChange(form.walletId)" style="width: 250px;">
                  <el-option v-for="item in this.walletList" :key="item.walletId" :label="item.name"  :value="item.walletId">
                  </el-option>
                </el-select>
                <el-button @click="$router.push({ name: 'digitalWallet' })" class="elbut">添加数字钱包</el-button>
              </el-form-item>
              <el-form-item label="提币数额：" prop="withdrawalAmount" >
                <el-input v-model="form.withdrawalAmount" @blur="calculationFee()"></el-input>
              </el-form-item>
              <el-form-item label="手续费：" prop="handingFee">
                <el-input v-model="form.handingFee" disabled="disabled"></el-input>
              </el-form-item>
               <el-form-item label="预计到账：" prop="delivery">
                 <el-input v-model="form.realWithdrawalAmount" disabled="disabled"></el-input>
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
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  import { throws } from 'assert';
  export default {
    data () {
      let checkAmount = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('请输入提币数额'))
        } else {
           const reg =  /\D/g
          if (reg.test(value)) {
             return callback(new Error('请输入数字'))
          }else if (value < 50) {
            return callback(new Error('提币数额不能低于50个'))
          } else {
            callback()
          }
        }
      }
      return {
        totalNumberYycAmount: '0.000000',
        totalNumberUsdtAmount: '0.000000',
         form: {
          walletId: '',
          accountType: '',
          wallet: '',
          walletAddress: '',
          handingFee: '',
          withdrawalAmount: '',
          realWithdrawalAmount: '',
          payPassword: ''
        },
        walletList: [],
        rules: {
          accountType: [
            { required: true, message: '请选择提币类型', trigger: 'change' }
          ],
          walletId: [
            { required: true, message: '请选择钱包', trigger: 'change' }
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
          url: this.$http.adornUrl('/fund/au/current/info'),
          method: 'get'
        }).then(({data}) => {
           console.log(data)
          if (data && data.code === '0000') {
            this.totalNumberYycAmount = data.data.yyc
            this.totalNumberUsdtAmount = data.data.usdt
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      calculationFee() {
        this.form.realWithdrawalAmount = (this.form.withdrawalAmount - this.form.withdrawalAmount*this.form.handingFee).toFixed(2);
      },
      submitForm (formName) {
        if(this.form.accountType == 1 &&  this.form.withdrawalAmount > this.totalNumberUsdtAmount){
            return  this.$message.error('USDT账户余额不足')
          }else if(this.form.accountType == 2 &&  this.form.withdrawalAmount > this.totalNumberYycAmount){
            return  this.$message.error('YYC账户余额不足')
          }
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/fund/au/insert'),
              method: 'post',
              data: this.$http.adornData({
                walletId : this.form.walletId,
                accountType : this.form.accountType,
                wallet: this.form.wallet,
                walletAddress: this.form.walletAddress,
                withdrawalAmount: this.form.withdrawalAmount,
                realWithdrawalAmount: this.form.realWithdrawalAmount,
                payPassword: this.form.payPassword
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
              } else {
                this.$message.error(data.msg)
              }
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      getWalletList(accountType){
        if(!accountType){return}
        this.$http({
          url: this.$http.adornUrl('/fund/au/wallet/list'),
          method: 'get',
          params: this.$http.adornParams({'walletType': accountType})
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.walletList = data.data.walletList
            this.form.handingFee = data.data.handingFee
          } else {
            this.walletList=[]
            this.$message.error(data.msg)
          }
        })
      },
      selectChange(id){
        if (this.walletList.length > 0) {
          for (let i = 0; i < this.walletList.length; i++) {
            if (id == this.walletList[i].walletId) {
              this.form.wallet = this.walletList[i].name
              this.form.walletAddress = this.walletList[i].address
              console.log(this.form.walletAddress+"---"+this.form.wallet)
            }
          }
        }
      },
      resetForm(form) {
        this.$refs[form].resetFields();
      }
    },
    mounted () {}
  }
</script>