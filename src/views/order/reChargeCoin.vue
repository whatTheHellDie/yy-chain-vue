<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="top-nav">
            <div aria-label="Breadcrumb" role="navigation" class="el-breadcrumb">
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">订单管理</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span>
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">充币订单</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span> <span class="el-breadcrumb__item bold" aria-current="page">
                <span role="link" class="el-breadcrumb__inner" style="font-weight: 800;">重新提交</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span></div>
          </div>
          <div class="box-body min425">
            <div class="charge-coin set-password">
              <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">转账数额</label><input type="text" class="form-control input" v-model="form.chargeAmount" placeholder="请输入转账LoveBird数额">
                </div>
                <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">交易号</label><input type="text" class="form-control input" v-model="form.chargeNumber" v-bind:value="form.chargeNumber" placeholder="请在钱包记录页面复制URL粘贴在这里">
                </div>

                <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">上传凭证</label>
                  <div class="col-sm-4 pin">
                    凭证为转账支付成功的订单详情图片，需包含以下四项内容：转账金额、转账用户钱包地址、收款方钱包地址、交易时间。
                    <div class="clearfix">
                      <div class="tou-s">
                        <img :src="this.form.chargeVoucher" class="img" />
                        <div class="btn-sub">上传图片<input @change='add_img($event,0)' id="saveImage" accept="image/png,image/jpeg,image/gif" type="file"></div>
                      </div>
                    </div>
                  </div>

                </div>
                <div class="gu-btn" @click="submitData()">提交</div>
            </div>
          </div>

        </div>
      </div>
    </main-body>
  </div>
</template>

<style lang="scss" scoped>
  .card-box {
    display: inline-block;
    vertical-align: top;
  }
  .btn-sub {
    position: relative;
    cursor: pointer;
    input {
      position: absolute;
      top: 0px;
      left: 0px;
      width: 100%;
      height: 100%;
      z-index: 99999;
      opacity: 0;
    }
  }
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
      return {
        imgs: ['/static/img/zhan.png'],
        copyMessage: "LjoeXwCevS6RxdBjfLKBF5EQDLT2B1uRMWsdfsdf",
        num1: 1,
        orderNumber:"",
        form: {
          chargeAmount: '',
          chargeNumber: '',
          chargeVoucher: ''
        }
      }
    },
    components: {
      MainBody
    },
    activated(){
        this.orderNumber = this.$route.query.orderNumber;
        this.form.chargeNumber = this.$route.query.chargeNumber;
        this.form.chargeVoucher = this.$route.query.chargeVoucher;
        this.form.chargeAmount  = this.$route.query.chargeAmount;
    },
    methods: {
      submitData () {
        if (!this.form.chargeAmount) {
          this.$alert('转账数额不能为空', '提示', {
            confirmButtonText: '确定'
          })
          return false
        }
        if (!this.form.chargeNumber) {
          this.$alert('交易号不能为空', '提示', {
            confirmButtonText: '确定'
          })
          return false
        }
        if (!this.form.chargeVoucher) {
          this.$alert('请上传图片', '提示', {
            confirmButtonText: '确定'
          })
          return false
        }
        this.$http({
          url: this.$http.adornUrl('/fund/au/save'),
          method: 'post',
          data: this.$http.adornData({
            'chargeAmount': this.form.chargeAmount,
            'chargeNumber': this.form.chargeNumber,
            'chargeVoucher': this.form.chargeVoucher,
            'number' : this.orderNumber
          })
        }).then(({
                   data
                 }) => {
          if(data && data.code === '0000') {
            this.$message({
              type: 'success',
              message: '上传成功'
            })
            location.reload()
          } else {
            this.$alert(data.msg, '提示', {
              confirmButtonText: '确定'
            });
          }
        })
      },
      add_img(event, index) {
        var reader = new FileReader();
        var img1 = event.target.files[0];
        if(window.sessionStorage.getItem("auth") == null) {
          this.$alert('用户未认证', '提示', {
            confirmButtonText: '确定',
          });
          return false
        }
        if(index == 0 && this.imgs.length > 0) {
          this.imgs.splice(0, 1);
        }
        if(index == 1 && this.imgs1.length > 0) {
          this.imgs1.splice(0, 1);
        }
        reader.readAsDataURL(img1);
        var that = this;
        reader.onloadend = function() {
          if(img1.size > 1048576) {
            that.$alert('图片不能大于1m', '提示', {
              confirmButtonText: '确定',
            });
            return false
          }

          that.imgs.push(reader.result)
          let x = document.getElementById('saveImage').files[0];
          let params = new FormData();
          params.append('fileName', x);
          // alert(that.$cookie.get('token'));
          let config = {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          };
          that.$axios.post(that.$http.adornUrl('/fund/au/fileUpload'), params, {
            headers: {
              "Content-Type": "multipart/form-data",
              'access_token': that.$cookie.get('yy-chain-token')
            }
          }).then(({
                     data
                   }) => {
            this.dataListLoading = false
            console.log(data)
            if(data && data.code === '0000') {
              that.form.chargeVoucher = data.data
            } else {
              that.$message.error(data.msg)
            }
          }).catch(({
                      error
                    }) => {
            that.dataListLoading = false
            that.$message.error(error)
          })
        }
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
