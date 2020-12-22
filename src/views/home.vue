<template>
  <div class="wraaper">
    <div class="header">
      <div class="wrraaper">
        <div class="logo"></div>
      </div>
    </div>
    <div class="modal">
      <div class="wrraaper">
        <div class="footer1">
          在线图片加密解密
        </div>
        <div class="footer2">
          <div class="wenzi1">
            <div>选择图片:</div>
            <div style="margin-right: 40%">Base64编码:</div>
          </div>
          <div class="shangchuan">
            <div class="tup">
              <el-upload
                class="avatar-uploader"
                action=""
                ref="uploadAvatar"
                :show-file-list="false"
                :auto-upload="false"
                :on-change="changeFile"
                list-type="picture">
                <img v-if="imageUrl" :src="imageUrl" class="uploadAvatar">
                <el-button size="small" type="primary" v-if="!imageUrl">选择图片</el-button>
              </el-upload>
            </div>
            <div class="bianma">
              <el-input
                type="textarea"
                :rows="12"
                placeholder="Base64编码"
                v-model="textarea">
              </el-input>
            </div>
          </div>
          <div class="tup">

          </div>
        </div>
        <div class="footer3">
          <div class="wenzi-wrapper">
            *请上传小于1MB的JPG 或 PNG 格式图片。
          </div>
          <div class="ann">
            <el-button size="small" type="primary" @click="zhuantup">图片转出Base64</el-button>
            <el-button size="small" type="primary" @click="zhuanbase">Base64还原成图片</el-button>
            <el-button size="small"  type="info" plain @click="qingk">清空结果</el-button>
          </div>
        </div>
      </div>
    </div>
    <div class="footer"></div>
  </div>
</template>



<script>
  export default {
    name: "home",
    data() {
      return {
        imageUrl: '',
        imageBaseUrl: '',
        textarea:''
      }
    },
    methods:{
      /**文件框改变事件  @param file  @param fileList */
      changeFile(file, fileList) {
        const isJPGORPNG = (file.raw.type === 'image/jpeg' || file.raw.type === 'image/png');
        const isLt1M = file.size / 1024 / 1024 < 1;

        if (!isJPGORPNG) {
          this.$message.info('上传图片只能是 JPG 或 PNG 格式!');
          return;
        }
        if (!isLt1M) {
          this.$message.info('上传图片大小不能超过 1MB!');
          return;
        }

        let that = this;
        let reader = new FileReader();
        reader.readAsDataURL(file.raw);
        reader.onload = function(e){
          this.result; //base64编码
          that.imageBaseUrl = this.result;
          that.imageUrl = this.result;
        }
      },
      base64ImgtoFile(dataurl, filename = 'file') {
        let arr = dataurl.split(',')
        let mime = arr[0].match(/:(.*?);/)[1]
        let suffix = mime.split('/')[1]
        let bstr = atob(arr[1])
        let n = bstr.length
        let u8arr = new Uint8Array(n)
        while (n--) {
          u8arr[n] = bstr.charCodeAt(n)
        }
        return new File([u8arr], `${filename}.${suffix}`, {
          type: mime
        })
      },
      zhuantup(){
        if (this.imageBaseUrl){
          this.textarea  = this.imageBaseUrl
        }else {
          // console.log(2121212)
          this.$message({
            message: '请选择一张图片',
            type: 'warning'
          });
        }
      },
      zhuanbase(){
        // console.log(this.textarea,1111)
        // let imgFile = this.base64ImgtoFile(this.textarea);
        // console.log(imgFile,3333)
        // this.imageUrl = imgFile.name
        // this.imageBaseUrl = imgFile.name
        if (this.textarea){
          this.imageBaseUrl = this.textarea
          this.imageUrl = this.textarea
        }else {
          this.$message({
            message: '请将Base64的编码复制在文本框内',
            type: 'warning'
          });
        }

      },
      qingk(){
        if (this.imageBaseUrl || this.textarea){
          this.textarea = ''
          this.imageBaseUrl = ''
          this.imageUrl = ''
        }else {
          this.$message({
            message: '已经清空',
            type: 'warning'
          });
        }

      }
    }
  }
</script>

<style scoped>
  .wraaper{  margin: 0 auto;min-width: 1200px; }
  .wrraaper{ width: 1200px;margin: 0 auto; }
  .header{ background-color: rgb(0,108,191);height: 60px }
  .logo{
    background: url('../assets/home/logo.gif') no-repeat;
    line-height: 60px;cursor: pointer;width: 250px;height: 60px;
  }
  .footer1{ margin-top: 18px; font-size: 24px ;color: #777  }
  .footer2{ margin-top: 20px }
  .wenzi1{ font-size: 13px;line-height: 18px;color: #333333;display: flex;justify-content: space-between }
  .shangchuan{
    margin-top: 20px;height: 300px;
    display: flex;justify-content: space-between;
  }
  .tup{
    border: 1px solid #777777;
    width: 49%;margin:0 auto;
    display: flex;justify-content:center;align-items: center;
  }
  .bianma{
    border: 1px solid #777777;
    width: 49%;display: flex;align-items: center }
  .wraaper >>> .el-upload{ margin: 0 auto }
  .uploadAvatar{ width: 50%; }
  .footer3{ display: flex;justify-content: space-between;margin-top: 5px;height: 50px }
  .wenzi-wrapper{ font-size: 14px;margin-left: 20px;color: #777777 ;width: 49%;line-height: 50px }
  .ann{ width: 49%;display: flex;justify-content: flex-end;align-items: center;margin-right: 20px }
</style>
