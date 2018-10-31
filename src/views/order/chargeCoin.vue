<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <h1>充币</h1>
          <div class="box-body">
            <div class="charge-coin">
              <div class="blue title">第一步：转账</div>
              <form class="form-horizontal" role="form">
                <div class="form-group combo-form clearfix">
                  <div class="way">方式一：扫描收款二维码
                    <div class="please">请用您的个人数字钱包扫描二维码进行转账（USDT）。<br>
                      <div class="erwei">

                      </div>
                    </div>
                  </div>
                  <div class="way">方式二：收款方钱包地址
                    <div class="please">请您复制收款方数字钱包地址，在您的数字钱包中进行转账支付（USDT）。<br>
                      <div class="copy text-right" v-clipboard:copy="copyMessage" v-clipboard:success="onCopy">复制钱包地址</div>
                      <div class="clear"></div>
                      <div class="wallet-address">
                        <div>钱包地址</div>
                        <div>{{copyMessage}}</div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="blue title" style="margin-top: 30px;">第二步 填写转账凭证</div>
                <form>
                  <div class="form-group combo-form">
                    <label for="zhuan" class="col-sm-2 control-label">转账数额</label><input type="text" class="form-control input" v-model="form.chargeAmount" placeholder="请输入转账USDT数额">
                  </div>
                  <div class="form-group combo-form">
                    <label for="zhuan" class="col-sm-2 control-label">交易号</label><input type="text" class="form-control input" v-model="form.chargeNumber" placeholder="请在钱包记录页面复制URL粘贴在这里">
                  </div>

                  <div class="form-group combo-form">
                    <label for="zhuan" class="col-sm-2 control-label">上传凭证</label>
                    <div class="col-sm-4 pin">
                      凭证为转账支付成功的订单详情图片，需包含以下四项内容：转账金额、转账用户钱包地址、收款方钱包地址、交易时间。
                      <div class="clearfix">
                        <div class="tou-s">
                          <img :src="imgs[0]" class="img" />

                          <div class="btn-sub">上传图片<input @change='add_img($event,0)' id="saveImage" accept="image/png,image/jpeg,image/gif" type="file"></div>
                        </div>
                      </div>
                    </div>

                  </div>
                </form>
                <div class="gu-btn" @click="submitData()">提交</div>

              </form>
            </div>
          </div>

        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
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
        form: {
          chargeAmount: '',
          chargeNumber: '',
          chargeVoucher: ''
        },
        btn:1
      }
    },
    components: {
      MainBody
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
        if (this.form.chargeNumber.length < 20) {
          this.$alert('交易号不能少于20字符', '提示', {
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
        if (this.btn == 0) {
          return false
        }
        this.btn == 0;
        this.$http({
          url: this.$http.adornUrl('/fund/au/charge'),
          method: 'post',
          data: this.$http.adornData({
            'chargeAmount': this.form.chargeAmount,
            'chargeNumber': this.form.chargeNumber,
            'chargeVoucher': this.form.chargeVoucher
          })
        }).then(({data}) => {
          this.btn == 1
          if (data && data.code === '0000') {
            this.$message({
              type: 'success',
              message: '充币订单提交成功'
            })
            location.reload()
          } else {
            this.$alert(data.msg, '提示', {
              confirmButtonText: '确定'
            })
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
              'ACCESS_TOKEN': that.$cookie.get('yy-chain-token')
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
      },
      onCopy(){
        this.$message({
          message: '复制成功',
          type: 'success'
        });
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
