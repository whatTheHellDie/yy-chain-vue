<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <h1>数字钱包管理&nbsp;&nbsp;<span class="h1span">最多保存20个数字钱包</span></h1>
          <div class="box-body">
            <div class="chargeCoin2">
              <el-form ref="form" :model="form" label-width="180px">
                <el-form-item label="数字钱包名称：">
                  <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="数字钱包类型：">
                  <el-select v-model="form.walletType" placeholder="请选择数字钱包类型">
                    <el-option label="区域一" value="shanghai"></el-option>
                    <el-option label="区域二" value="beijing"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="数字钱包地址：">
                  <el-input v-model="form.address" placeholder="请输入数字钱包地址"></el-input>
                </el-form-item>
                <el-form-item label="手机号码：">
                  <el-input v-model="form.phone" placeholder="请输入手机号"></el-input>
                </el-form-item>
                <el-form-item>
                  <el-button type="primary" @click="onSubmit">保存</el-button>
                </el-form-item>
              </el-form>
              <el-table :data="tableData" border>
                <el-table-column prop="name" label="数字钱包名称" >
                </el-table-column>
                <el-table-column prop="walletType" label="数字钱包类型">
                  <template slot-scope="scope">
                    <div v-if="scope.row.accountType == 1">USDT</div>
                    <div v-else-if="scope.row.accountType == 2">YYC</div>
                    <div v-else-if="scope.row.accountType == 3">易用积分</div>
                  </template>
                </el-table-column>
                <el-table-column prop="address" label="数字钱包地址" width="350">
                </el-table-column>
                <el-table-column prop="phone" label="手机号码">
                </el-table-column>
                <el-table-column prop="balance" label="操作">
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
    color: cadetblue;
  }
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        tableData: [],
         form: {
          name: '',
          walletType: '',
          address: '',
          phone: ''
        },
        chooseContent: []
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
            this.elementTotal = data.data.pageResponse.elementTotal
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
      }
    },
    mounted () {
    }
  }
</script>
