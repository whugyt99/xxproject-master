<template>
  <div>
    <div style="margin-left:170px;margin-right:170px;margin-top:20px">
      <span>{{id}}缺陷检测</span>

    </div>
    <div style="margin-left:170px;margin-right:170px;margin-top:20px">
      <el-upload
        class="upload-demo" name="photo"
        ref="upload"
        :action="UploadUrl()"
        :on-preview="handlePreview"
        :on-remove="handleRemove"
        :file-list="fileList"
        list-type="picture"
        :auto-upload="false"
        :on-success="handleAvatarSuccess"
      >
        <el-button slot="trigger" size="small" type="primary">选取图片</el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">开始检测</el-button>
        <div>
          <span>your k:</span>
          <el-input
            style="margin-top: 10px;margin-left: 10px; max-width=100px"
            placeholder="请输入内容"
            v-model="k"
            clearable>
          </el-input>
        </div>
      </el-upload>
      <!--  检测结果 -->
      <div style="height: 100px">
        <div v-for="n in num" style="margin:10px 0 0 0;">{{result[n-1]}}</div>
      </div>

    </div>
  </div>
</template>
<script>
  export default {

    name: 'DefectDetectionPage',
    data() {
      return {
        id: '',
        value: '',
        fileList: [],
        pic_class: '',
        url: '',
        result: [],
        num: 0,
        //label: '',
        k: '',
      }
    },
    mounted() {
      this.getParams();
      //this.UploadUrl();
    },
    methods: {
      getParams() {
        // 取到路由带过来的参数
        this.id = this.$route.query.id;
        console.log(this.id);
        this.value = this.$route.query.value;
        console.log(this.value);
      },
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },

      handlePreview(file) {
        console.log(file);
      },
      //图片上传
      submitUpload() {
        //let _this = this;
//setTimeout(function() {
        //_this.$refs.upload.submit();
        //},400)
        this.$refs.upload.submit();
      },
      //图片上传成功后
      handleAvatarSuccess(response, file, fileList) {
        //response
        console.log(response);
        if(response.state === 200){
          if(response.data === 0){
  this.result[this.num] = "检测结果: 检测成功，此图片是缺陷图片！";
  }else{
  this.result[this.num] = "检测结果: 检测成功，此图片是正常图片！";
  }
  }else{
  this.result[this.num] = "检测失败，图片类型错误！";
  }
        //this.result[this.num] = "检测结果: " + response.msg;
        this.num = this.num + 1;
        console.log(this.num);
        //console.log(this.result[0]);
      },
//      beforeUpload(file) {
//        return new Promise((resolve, reject) => {
//          //let _URL = window.URL || window.webkitURL;
//          let isLt2M = file.size / 1024 / 1024 < 2; // 判定图片大小是否小于2MB
//          if (isLt2M) {
//            resolve(file)
//          } else {
//            console.log('111');
//            const imageConversion = require("image-conversion");
//            //const es6promise = require('es6-promise').polyfill();
//            //console.log(file); // 压缩到400KB,这里的400就是要压缩的大小,可自定义
//            imageConversion.compressAccurately(file, 2048).then(res => {
//              console.log(res);
//              console.log('222');
//              resolve(res);
//            }).catch((error) => {
//              reject(error);
//              //throw new Error(error);
//              //console.log(error);
//            })
//          }
//          //img.src = _URL.createObjectURL(file);
//        })
//      },
      UploadUrl: function () {
        return "http://47.98.232.219:5000/upload_pic?pic_class=" + this.value;
      }
    }
  }
</script>
<style>

</style>
