<template>
  <div class="body-grey">
    <main-body>
      <div slot="content">
        <div class="box-card2">
          <div class="top-nav">
            <div aria-label="Breadcrumb" role="navigation" class="el-breadcrumb">
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">账户中心</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span>
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">个人信息</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span> <span class="el-breadcrumb__item bold" aria-current="page">
                <span role="link" class="el-breadcrumb__inner" style="font-weight: 800;">实名认证</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span></div>
          </div>
          <div class="box-body min425">
            <form class="form-horizontal" role="form">
          <div class="form-group combo-form">
            <label for="zhuan" class="col-sm-3 control-label red">认证不通过原因：</label>
            <div class="lh40 red">填写姓名与证件图片中的姓名不一致</div>
          </div>
          <div class="form-group combo-form">
            <label for="zhuan" class="col-sm-3 control-label">真实姓名</label>
            <div class="col-sm-4">
              <input class="form-control input" placeholder="请输入真实姓名">
            </div>
            <div class="tips lh40 red">* 只包含字母和中文,并且不能低于两位</div>
          </div>
          <div class="form-group combo-form">
            <label for="zhuan" class="col-sm-3 control-label">身份证号</label>
            <div class="col-sm-4">
              <input class="form-control input" placeholder="请输入身份证号">
            </div>
            <div class="tips lh40 red">* 所输入的身份证号不能低于十六位</div>
          </div>
          <div class="form-group combo-form">
            <label for="zhuan" class="col-sm-3 control-label">上传证件图片:</label>
            <div class="col-sm-8">
              <div class="card-box mr45">
                <div class="card-wrap">
                  <img src="/static/img/zhan.png" class="img">
                  <div class="btn btn-chuan">上传图片</div>
                  <div class="card-tip">上传的身份证照片必须与上面填写的身份信息一致 ，否则申请不能通过。
                  </div>
                </div>
                <div class="text-center">（证件正面）</div>
              </div>
              <div class="card-box">
                <div class="card-wrap">
                  <img src="/static/img/zhan.png" class="img">
                  <div class="btn btn-chuan">点击上传</div>
                  <div class="card-tip">上传的身份证照片必须与上面填写的身份信息一致，否则申请不能通过。</div>
                </div>
                <div class="text-center">（证件背面）</div>
              </div>
            </div>
          </div>

          <div class="form-group">
            <div class="col-sm-offset-3 col-sm-9">
              <div class="btn-ti">提交</div>
            </div>
          </div>

        </form>
          </div>

        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
  .col-sm-3{margin-right: 10px;width: 141px;text-align: right;display: inline-block;vertical-align: top;line-height: 35px;}
  .col-sm-4{width: 371px;line-height: 35px;
  display: inline-block;vertical-align: top;
    input{width: 100%;height: 35px;padding-left: 11px;}
  }
  .col-sm-8{
    display: inline-block;vertical-align: top;
  }
  .combo-form{margin-bottom: 30px;}
  .lh40{
    line-height: 35px;
    display: inline-block;vertical-align: top;
  }
  .red{color: #f43837;}
  .tips{margin-left: 6px;}
  .card-box{display: inline-block;vertical-align: top;
    &.mr45{margin-right: 45px;}
  }
  .card-wrap{width: 310px;border: 1px solid #d1d1d1;margin-bottom: 6px;display: inline-block;vertical-align: top;
  
    .img{display: block;margin: 20px auto;width:267px;height: 200px;}
    .card-tip{
        margin: 10px 40px;
        color: #999;
    }
    .mr45{margin-right: 45px;}
    }
    .btn-chuan{
        width: 150px;
        height: 30px;
        line-height: 30px;
        border:1px #ED9D0E solid;
        padding: 0;
        color: #ED9D0E;
        display: block; 
        margin: 0 auto;
        text-align: center;
        border-radius: 5px;cursor: pointer;
        
    }
    .btn-ti{
        width: 121px;
    height: 36px;
    line-height: 36px;
    background: #000;
    color: #fff;
    text-align: center;
    border-radius: 5px;
    margin-left: 154px;
    margin-bottom: 10px;
    }
</style>

<script>
  //import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
      return {
      }
    },
    components: {
      MainBody
    },
  }
</script>