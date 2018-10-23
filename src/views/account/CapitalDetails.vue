<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-list">
              <div class="choose-box">
                <div class="choose-list">
                  <div v-for="item,i in chooseList" class="box box2" @click="loadList(i)" :class="{active:i == activeNumber}"><span class="span">{{item.name}}</span></div>
                </div>
                <div class="keyong">
                  <div class="number">0</div>
                  <div class="name">可用余额</div>
                </div>
                <div class="zijin-form">
                  <el-form ref="form" :inline="true" :model="form" label-width="80px">
                    <label for="">分类 </label>
                    <el-form-item>
                      <el-select v-model="form.region" placeholder="全部状态">
                        <el-option label="区域一" value="shanghai"></el-option>
                        <el-option label="区域二" value="beijing"></el-option>
                      </el-select>
                    </el-form-item>
                    <label for="">时间</label>
                    <el-form-item>
                      <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                    </el-form-item>
                    <el-form-item>
                      <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                    </el-form-item>
                    <el-form-item>
                      <div class="gu-btn">搜索</div>
                    </el-form-item>
                  </el-form>
                  <el-table :data="tableData" border>
                    <el-table-column prop="date" label="时间" width="157">
                    </el-table-column>
                    <el-table-column prop="name" label="账户类型" width="86">
                    </el-table-column>
                    <el-table-column prop="address" label="资金变动类型" width="108">
                      </el-table-column>
                      <el-table-column prop="date1" label="详细说明" width="309 ">
                      </el-table-column>
                      <el-table-column prop="name1" label="变动金额" width="121">
                      </el-table-column>
                      <el-table-column prop="address1" label="账户可用余额" width="127">
                      </el-table-column>
                  </el-table>
                </div>
              </div>
            </div>
            <div class="m-page mb40">
              <ul class="pagination">
                <li>
                  <a href="javascript:void(0)">&lt;</a>
                </li>
                <li class="active">
                  <a href="javascript:void(0)">1</a>
                </li>
                <li>
                  <a href="javascript:void(0)">&gt;</a>
                </li>
              </ul>
              <div class="turn-to">
                跳至<input type="text" value="1">页
                <a href="javascript:void(0)" class="btn btn-turn">跳转</a>
              </div>
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
  //import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
      return {
        activeNumber: 0,
        chooseList: [{
            name: 'USDT'
          },
          {
            name: '易用积分'
          },
          {
            name: 'YYC'
          },
        ],
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        tableData: [],
        
      }
    },
    components: {
      MainBody
    },
    methods: {
      loadList(i) {
        this.activeNumber = i;
      },
      pay(id) {

      },
      cancelOrder(id) {

      },
      delOrder(id) {

      }
    },
    mounted() {
      //    var clipboard = new ClipboardJS('.copy');
      //
      //    clipboard.on('success', function(e) {
      //        e.clearSelection();
      //        
      //    });
      //    
      //    clipboard.on('error', function(e) {
      //        alert('该默认浏览器不支持点击复制,请长按选择复制钱包地址或选择分享二维码图片')
      //    });
    }
  }
</script>