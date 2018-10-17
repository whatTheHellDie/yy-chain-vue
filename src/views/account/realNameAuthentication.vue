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
              <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span>
            </div>
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
                      <img :src="imgs[0]" class="img">
                      <div class="btn btn-chuan">上传图片 <input @change='add_img($event,0)'  type="file"></div>
                      <div class="card-tip">上传的身份证照片必须与上面填写的身份信息一致 ，否则申请不能通过。
                      </div>
                    </div>
                    <div class="text-center">（证件正面）</div>
                  </div>
                  <div class="card-box">
                    <div class="card-wrap">
                      <img :src="imgs1[0]" class="img">
                      <div class="btn btn-chuan">点击上传 <input @change='add_img($event,1)'  type="file"></div>
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


<script>
  //import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
       return{
                    imgs:['/static/img/zhan.png'],
                    imgs1:['/static/img/zhan.png'],
              }
    },
     computed:{
    },
    components: {
      MainBody,
    },
    methods:{
       delete_img(item){
               this.imgs.splice(item,1);
        },
        add_img(event,index){
          var reader =new FileReader();
             var img1=event.target.files[0];
             
           if(!/image\/\w+/.test(img1.type)){ 
              alert("文件必须为图片！"); 
              return false; 
          } 
          if(index==0&&this.imgs.length>0){
            this.imgs.splice(0,1);
          }
          if(index==1&&this.imgs1.length>0){
            this.imgs1.splice(0,1);
          }
            
                  
                   reader.readAsDataURL(img1);
                   var that=this;
                   reader.onloadend=function(){
                     if (img1.size > 102400) {
                                that.$alert('图片不能大于1m', '提示', {
                                confirmButtonText: '确定',
                                callback: action => {
                                 
                                }
                              });
                              return false
                     }
                     alert(index)
                     if(index==0){
                       that.imgs.push(reader.result)
                     }else{
                        that.imgs1.push(reader.result)
                     }
                   }
                  
        }


    }
  }
</script>

<style lang="scss" scoped>
.finish_room{
      width: 430px;
      height: auto;
  }

     .finish_room2{
     width: 100%;
     height: auto;
     padding-top: 15px;
     padding-bottom: 15px;
     display: flex;
     align-items: center;
      border-bottom: 2px solid #e1e1e1;
   }
    
   .finish_room2 .room_img{
     width: 150px;
     height: 100px;
     margin-right: 10px;
     position: relative;
     overflow: hidden;
   }
   .finish_room2 .room_img img{
     
     width: 100%;
     height: 100%;
   }
   .finish_room2>.room_img span{
      position: absolute;
      width: auto;
      height: auto;
      right: 5px;
      bottom:3px;
   }
 


   .room_add_img{
     width: 148px;
     height: 98px;
     border:1px solid #e1e1e1;
     display: flex;
     flex-direction: column;
     align-items: center;
     justify-content: space-between;
     position: relative;
   }
   .room_add_img >span:nth-child(1){
     margin-top: 20px;
     width: 40px;
     height: 40px;
     overflow: hidden;
   }
   .room_add_img >span:nth-child(2){
      margin-bottom: 10px;
   }

  .btn-chuan input,.room_add_img input{
      position: absolute;
      top: 0px;
      left: 0px;
      width: 100%;
      height: 100%;
      z-index: 99999;
      opacity: 0;
  }











.upload-button{  height: 60px; width: 60px;background: red}
  .col-sm-3 {
    margin-right: 10px;
    width: 141px;
    text-align: right;
    display: inline-block;
    vertical-align: top;
    line-height: 35px;
  }
  
  .col-sm-4 {
    width: 371px;
    line-height: 35px;
    display: inline-block;
    vertical-align: top;
    input {
      width: 100%;
      height: 35px;
      padding-left: 11px;
    }
  }
  
  .col-sm-8 {
    display: inline-block;
    vertical-align: top;
  }
  
  .combo-form {
    margin-bottom: 30px;
  }
  
  .lh40 {
    line-height: 35px;
    display: inline-block;
    vertical-align: top;
  }
  
  .red {
    color: #f43837;
  }
  
  .tips {
    margin-left: 6px;
  }
  
  .card-box {
    display: inline-block;
    vertical-align: top;
    &.mr45 {
      margin-right: 45px;
    }
  }
  
  .card-wrap {
    width: 310px;
    border: 1px solid #d1d1d1;
    margin-bottom: 6px;
    display: inline-block;
    vertical-align: top;
    .img {
      display: block;
      margin: 20px auto;
      width: 267px;
      height: 200px;
    }
    .card-tip {
      margin: 10px 40px;
      color: #999;
    }
    .mr45 {
      margin-right: 45px;
    }
  }
  
  .btn-chuan {
    position: relative;
    width: 150px;
    height: 30px;
    line-height: 30px;
    border: 1px #ED9D0E solid;
    padding: 0;
    color: #ED9D0E;
    display: block;
    margin: 0 auto;
    text-align: center;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .btn-ti {
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