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
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
              <el-form-item label="提币类型：" prop="region">
                <el-select v-model="ruleForm.region" >
                  <el-option label="区域一" value="shanghai"></el-option>
                  <el-option label="区域二" value="beijing"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="数字钱包：" prop="region">
                <el-select v-model="ruleForm.region" >
                  <el-option label="区域一" value="shanghai"></el-option>
                  <el-option label="区域二" value="beijing"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="提币数额：" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
              </el-form-item>
              <el-form-item label="手续费：" prop="delivery">
              </el-form-item>
               <el-form-item label="预计到账：" prop="delivery">
              </el-form-item>
              <el-form-item label="支付密码：" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        totalNumberYycAmount: '0.000000',
        totalNumberUsdtAmount: '0',
        activeNumber: 0,
        chooseContent: [],
         ruleForm: {
          name: '',
          region: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        rules: {
          name: [
            { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          region: [
            { required: true, message: '请选择活动区域', trigger: 'change' }
          ],
          type: [
            { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
          ],
          resource: [
            { required: true, message: '请选择活动资源', trigger: 'change' }
          ],
          desc: [
            { required: true, message: '请填写活动形式', trigger: 'blur' }
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
      loadList (i) {
        this.activeNumber = i
        this.$http({
          url: this.$http.adornUrl('/shares/order/information'),
          method: 'post',
          data: this.$http.adornData({
            'status': 1
          })
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === '0000') {
            this.totalNumberYyiAmount = data.data.totalNumberYyiAmount
            this.totalNumberOrder = data.data.totalNumberOrder
            this.totalNumberUsdtAmount = data.data.totalNumberUsdtAmount
            this.chooseContent = data.data.pageResponse.dataList
          } else {
            this.dataList = []
            this.elementTotal = 0
          }
          this.dataListLoading = false
        })
      },
      pay (id) {
      },
      cancelOrder (id) {
      },
      delOrder (id) {
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    },
    mounted () {
    }
  }
</script>
