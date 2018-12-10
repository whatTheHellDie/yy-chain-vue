<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <h1>数字钱包&nbsp;&nbsp;<span class="h1span">（最多保存20个数字钱包）</span></h1>
          <div class="box-body">
            <div class="chargeCoin2">
              <el-form ref="form" :model="form" :rules="formxRules" label-width="180px">
                <input type="hidden" v-model="form.walletId" >
                <el-form-item label="数字钱包名称："  prop="name">
                  <el-input v-model="form.name"  placeholder="请输入数字钱包名称"></el-input>
                </el-form-item>
                <el-form-item label="数字钱包类型："  prop="walletType">
                  <el-select v-model="form.walletType" placeholder="请选择数字钱包类型">
                    <el-option v-for="item in options2"
                      :key="item.value"
                      :label="item.label"  :value="item.value"  :disabled="item.disabled">
                    </el-option>
                    <!-- <el-option :label="USDT" :value="1"></el-option>
                    <el-option :label="YYC" :value="2"></el-option> -->
                  </el-select>
                </el-form-item>
                <el-form-item label="数字钱包地址：" prop="address">
                  <el-input v-model="form.address" placeholder="请输入数字钱包地址"></el-input>
                </el-form-item>
                <el-form-item label="手机号码：" prop="phone">
                  <el-input v-model="form.phone" placeholder="请输入手机号"></el-input>
                </el-form-item>
                <el-form-item>
                  <el-button type="primary" @click="onSubmit('form',form.walletId)">保存</el-button>
                </el-form-item>
              </el-form>
              <el-table :data="tableData" border>
                <el-table-column prop="name" label="数字钱包名称" >
                </el-table-column>
                <el-table-column prop="walletType" label="数字钱包类型">
                  <template slot-scope="scope">
                    <div v-if="scope.row.walletType == 1">USDT</div>
                    <div v-else-if="scope.row.walletType == 2">YYC</div>
                  </template>
                </el-table-column>
                <el-table-column prop="address" label="数字钱包地址" width="350">
                </el-table-column>
                <el-table-column prop="phone" label="手机号码">
                </el-table-column>
                <el-table-column  header-align="center" align="center" label="操作">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="updateHandle(scope.row.walletId)">修改</el-button>
                    <el-button type="text" size="small" @click="deleteHandle(scope.row.walletId,scope.row.name)">删除</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>
          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
  .h1span{
    font-size: xx-small;
    font-family: -webkit-pictograph;
    color: #ED9D0E;
  }
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      var checkPhone = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('手机号不能为空'))
        } else {
          const reg = /^1[3|4|5|7|8|6|9][0-9]\d{8}$/
          console.log(reg.test(value))
          if (reg.test(value)) {
            callback()
          } else {
            return callback(new Error('请输入正确的手机号'))
          }
        }
      }
      return {
        tableData: [],
        options2: [{
          value: '1',
          label: 'USDT'
        }, {
          value: '2',
          label: 'YYC'
        }],
        form: {
          walletId: '',
          name: '',
          walletType: '',
          address: '',
          phone: ''
        },
        formxRules: {
          walletType: [{ required: true, message: '请选择钱包类型', trigger: 'blur' }],
          phone: [{ required: true, validator: checkPhone, trigger: 'blur' }],
          name: [{ required: true, message: '钱包名称不能为空', trigger: 'blur' }],
          address: [{ required: true, message: '钱包地址不能为空', trigger: 'blur' }]
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
          url: this.$http.adornUrl('/wallet/au/list'),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.tableData = data.data
          } else {
            this.tableData = []
            this.$message.error(data.msg)
          }
        })
      },
      resetForm () {
        this.form.walletId = ''
        this.form.name = ''
        this.form.walletType = ''
        this.form.address = ''
        this.form.phone = ''
      },
      // 保存
      onSubmit (formName, walletId) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            if (!walletId) {
              this.$http({
                url: this.$http.adornUrl('/wallet/au/insert'),
                method: 'post',
                data: this.$http.adornData({
                  name: this.form.name,
                  walletType: this.form.walletType,
                  phone: this.form.phone,
                  address: this.form.address
                })
              }).then(({data}) => {
                if (data && data.code === '0000') {
                  this.$message({
                    message: '操作成功',
                    type: 'success',
                    duration: 1500,
                    onClose: () => {
                      this.loadList()
                      this.resetForm()
                    }
                  })
                } else if (data.code === '0001') {
                  this.$message({
                    message: '最多保存20个数字钱包',
                    type: 'faild',
                    duration: 1500,
                    onClose: () => {
                      this.loadList()
                      this.resetForm()
                    }
                  })
                } else {
                  this.$message.error(data.msg)
                }
              })
            } else {
              if (this.form.walletType == "USDT"){
                this.form.walletType = 1
              } else if (this.form.walletType == "YYC"){
                this.form.walletType = 2
              }
              this.$http({
                url: this.$http.adornUrl('/wallet/au/update'),
                method: 'post',
                data: this.$http.adornData({
                  walletId: this.form.walletId,
                  name: this.form.name,
                  walletType: this.form.walletType,
                  phone: this.form.phone,
                  address: this.form.address
                })
              }).then(({data}) => {
                if (data && data.code === '0000') {
                  this.$message({
                    message: '操作成功',
                    type: 'success',
                    duration: 1500,
                    onClose: () => {
                      this.loadList()
                      this.resetForm()
                    }
                  })
                } else {
                  this.$message.error(data.msg)
                }
              })
            }
          } else {
            console.log('error submit!!')
            return false
          }
        })
      },
      // 详情 / 修改页面
      updateHandle (id) {
        this.$http({
          url: this.$http.adornUrl('/wallet/au/detail'),
          method: 'get',
          params: this.$http.adornParams({'walletId': id})
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.form.walletId = data.data.walletId
            this.form.name = data.data.name
            if (data.data.walletType == "1") {
              this.form.walletType = "USDT"
            } else {
              this.form.walletType = "YYC";
            }
            this.form.phone = data.data.phone
            this.form.address = data.data.address
          } else {
            this.resetForm()
            this.$message.error(data.msg)
          }
        })
      },
      // 删除
      deleteHandle (id, name) {
        debugger
        this.$confirm(`确定${id ? '删除' : '批量删除'}【${name}】钱包吗?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning',
          center: true
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/wallet/au/delete'),
            method: 'delete',
            params: this.$http.adornParams({'walletId': id})
          }).then(({data}) => {
            if (data && data.code === '0000') {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.loadList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        }).catch(() => {})
      }
    },
    mounted () {}
  }
</script>
