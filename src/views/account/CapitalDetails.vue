<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-list">
              <div class="choose-box">
                <div class="choose-list">
                  <!--<div v-for="item,i in chooseList" class="box box2" @click="loadList(i)"
                       :class="{active:i == activeNumber}">
                    <span class="span">{{item.name}}</span>
                  </div>-->
                  <div class="box box2" @click="tabClick(1)" :class="{active:1 == activeNumber}">
                    <span class="span">USDT</span>
                  </div>
                  <div class="box box2" @click="tabClick(3)" :class="{active:3 == activeNumber}">
                    <span class="span">易用积分</span>
                  </div>
                  <div class="box box2" @click="tabClick(2)" :class="{active:2 == activeNumber}">
                    <span class="span">YYC</span>
                  </div>
                </div>
                <div class="keyong">
                  <div class="number">{{accountBalance}}</div>
                  <div class="name">可用余额</div>
                </div>
                <div class="zijin-form">
                  <el-form ref="form" :inline="true" :model="form" label-width="80px">
                    <label for="">分类 </label>
                    <el-form-item>
                      <el-select v-model="form.region">
                        <el-option v-for="option in typeOptions" :key="option.value" :label="option.label"
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
                     <!-- <div class="gu-btn">搜索</div>-->
                      <el-button type="primary" @click="loadList()">搜索</el-button>
                    </el-form-item>
                  </el-form>
                  <el-table :data="tableData" border>
                    <el-table-column prop="createTime" label="时间" width="155">
                    </el-table-column>
                    <el-table-column prop="accountType" label="账户类型" width="80">
                      <template slot-scope="scope">
                        <div v-if="scope.row.accountType == 1">USDT</div>
                        <div v-else-if="scope.row.accountType == 2">YYC</div>
                        <div v-else-if="scope.row.accountType == 3">易用积分</div>
                      </template>
                    </el-table-column>
                    <el-table-column prop="recordFirstType" label="分类" :formatter="firstType" width="80">
                      </el-table-column>
                      <el-table-column prop="remark" label="详细说明" width="350">
                      </el-table-column>
                      <el-table-column prop="recordAmount" label="变动金额">
                      </el-table-column>
                      <el-table-column prop="balance" label="账户可用余额">
                      </el-table-column>
                  </el-table>
                </div>
              </div>
            </div>

            <div class="block" style="text-align: center;margin: 30px 0 40px">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                background=""
                :current-page="pageNum"
                :page-size="pageSize"
                layout="prev, pager, next, jumper"
                :total="elementTotal">
              </el-pagination>
            </div>
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
        activeNumber: 3,
        pageNum: 1,
        pageSize: 8,
        elementTotal: 0,
        accountBalance: 0,
        form: {
          name: '',
          region: '',
          createTime: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        typeOptions: [
          {value: '', label: '全部'},
          {value: '1', label: '入股'},
          {value: '2', label: '奖励'},
          {value: '3', label: '挂卖'},
          {value: '4', label: '销毁'},
          {value: '5', label: '释放'},
          {value: '6', label: '资产分配'},
          {value: '7', label: '充币'},
          {value: '8', label: '提币'},
          {value: '9', label: '转账'}
        ],
        tableData: []
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
        if (i) {
          this.activeNumber = i
        }
        if (!this.form.createTime) {
          this.form.createTime = []
        }
        this.$http({
          url: this.$http.adornUrl('/fund/record/list'),
          method: 'post',
          data: this.$http.adornData({
            'pageNum': this.pageNum,
            'pageSize': this.pageSize,
            'createEndTime': this.form.createTime.length === 2 ? this.form.createTime[1] : '',
            'createStartTime': this.form.createTime.length >= 1 ? this.form.createTime[0] : '',
            'accountType': this.activeNumber,
            'recordFirstType': this.form.region
          })
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === '0000') {
            this.tableData = data.data.dataList
            this.elementTotal = data.data.elementTotal
            console.log(data.data.pageSize)
          } else {
            this.dataList = []
            this.elementTotal = 0
          }
          this.dataListLoading = false
        })
      },
      queryBalance (i) {
        this.$http({
          url: this.$http.adornUrl('/fund/au/person'),
          method: 'get'
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === '0000') {
            if (i == 1) {
              this.accountBalance = data.data.usdt
            } else if (i == 2) {
              this.accountBalance = data.data.yyc
            } else if (i == 3) {
              this.accountBalance = data.data.yyi
            }
          } else {
            this.accountBalance = 0
          }
          this.dataListLoading = false
        })
      },
      tabClick (i) {
        this.loadList(i)
        this.queryBalance(i)
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
      pay (id) {},
      cancelOrder (id) {},
      delOrder (id) {},
      firstType (row, column, cellValue) {
        if (this.typeOptions.length > 0) {
          for (let i = 0; i < this.typeOptions.length; i++) {
            if (cellValue == this.typeOptions[i].value) {
              return this.typeOptions[i].label
            }
          }
        }
      }
    },
    mounted () {
      this.tabClick(this.activeNumber)
    }
  }
</script>
