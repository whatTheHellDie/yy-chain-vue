<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <h1>提币订单</h1>
          <div class="box-body no-padding">
            <div class="zijin-form">
                  <el-form ref="form" :inline="true" :model="form" label-width="80px">
                    <label for="">提币类型 </label>
                    <el-form-item>
                      <el-select v-model="form.accountType">
                        <el-option v-for="option in typeOptions" :key="option.value" :label="option.label"
                                   :value="option.value"></el-option>
                      </el-select>
                    </el-form-item>
                    <label for="">状态 </label>
                    <el-form-item>
                      <el-select v-model="form.status">
                        <el-option v-for="option in statusOptions" :key="option.value" :label="option.label"
                                   :value="option.value"></el-option>
                      </el-select>
                    </el-form-item>
                    <label for="">时间</label>
                    <el-form-item prop="createTime">
                      <el-date-picker type="daterange" placeholder="选择日期" v-model="form.createTime"
                                      style="width: 100%;margin-top:2px;"
                                      unlink-panels="true" value-format="yyyy-MM-dd">
                      </el-date-picker>
                    </el-form-item>
                    <el-form-item>
                      <el-button type="primary" @click="loadList()">查询</el-button>
                    </el-form-item>
                  </el-form>
                  <el-table :data="tableData" border>
                    <el-table-column prop="number" label="订单编号">
                    </el-table-column>
                    <el-table-column prop="accountType" label="提币类型">
                      <template slot-scope="scope">
                        <div v-if="scope.row.accountType == 1">USDT</div>
                        <div v-else-if="scope.row.accountType == 2">YYC</div>
                      </template>
                    </el-table-column>
                    <el-table-column prop="wallet" label="数字钱包名称">
                    </el-table-column>
                    <el-table-column prop="walletAddress" label="数字钱包地址">
                    </el-table-column>
                    <el-table-column prop="status" label="状态" :formatter="firstType" >
                    </el-table-column>
                    <el-table-column prop="withdrawalAmount" label="提币数额">
                    </el-table-column>
                    <el-table-column prop="handingFee" label="手续费">
                    </el-table-column>
                    <el-table-column prop="realWithdrawalAmount" label="预计到账">
                    </el-table-column>
                    <el-table-column prop="createTime" label="申请时间">
                    </el-table-column>
                    <el-table-column prop="checkTime" label="处理时间">
                    </el-table-column>
                  </el-table>
                </div>
            <div class="block" style="text-align: center;margin: 30px 0 40px">
              <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
                background="" :current-page="pageNum" :page-size="pageSize" layout="prev, pager, next, jumper" :total="elementTotal">
              </el-pagination>
            </div>

          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
  .bechoose-list .gradient {
    background-image: url(/static/img/gradient2.png);
  }
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        tableData: [],
        pageNum: 1,
        pageSize: 8,
        elementTotal: 0,
        form: {
          accountType: '',
          status: '',
          createTime: '',
          resource: '',
          desc: ''
        },
        typeOptions: [
          {value: '', label: '全部'},
          {value: 1, label: 'USDT'},
          {value: 2, label: 'YYC'}
        ],
        statusOptions: [
          {value: '', label: '全部'},
          {value: 0, label: '待处理'},
          {value: 1, label: '处理中'},
          {value: 2, label: '提币成功'},
          {value: 3, label: '提币失败'}
        ]
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
        if (!this.form.createTime) {
          this.form.createTime = []
        }
        this.$http({
          url: this.$http.adornUrl('/fund/au/page'),
          method: 'post',
          data: this.$http.adornData({
            'pageNum': this.pageNum,
            'pageSize': this.pageSize,
            'createEndTime': this.form.createTime.length === 2 ? this.form.createTime[1] : '',
            'createStartTime': this.form.createTime.length >= 1 ? this.form.createTime[0] : '',
            'accountType': this.form.accountType,
            'status': this.form.status
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            console.log(data)
            this.tableData = data.data.dataList
            this.elementTotal = data.data.elementTotal
          } else {
            this.dataList = []
            this.elementTotal = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      handleSizeChange (val) {
        this.pageSize = val
        this.pageNum = 1
        this.loadList()
      },
      // 当前页
      handleCurrentChange (val) {
        this.pageNum = val
        this.loadList()
      },
      firstType (row, column, cellValue) {
        if (this.statusOptions.length > 0) {
          for (let i = 0; i < this.statusOptions.length; i++) {
            if (cellValue === this.statusOptions[i].value) {
              return this.statusOptions[i].label
            }
          }
        }
      }
    },
    mounted () { }
  }
</script>
