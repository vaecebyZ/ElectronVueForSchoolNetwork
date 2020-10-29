<style src="../../../static/css/bootstrap.min.css"></style>
<style src="../../../static/css/bootstrap-theme.min.css"></style>
<style scoped>

</style>

<template>
  <div>
    <el-button @click.prevent="">下一条</el-button>
    <br>
      <h4><span class="yiyan"></span></h4>
  </div>
</template>


<script>
  import UidPwd from '../../../static/js/data/UidPwd';
  import Typed from 'typed.js';
  export default {
    data() {
      return {
        user: '',
        pwd: '123'
      }
    },
    methods: {
      netWorkCheck() {
          let i = (Math.ceil(Math.random() * 2706));
          this.user = UidPwd.User[i].name;
          this.pwd = UidPwd.User[i].pwd;
          //window.alert('设备在线');
          this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {
            let data = res.data.data;
            let conte = [];
            conte.push(data.constant + " ---" + data.source)
            var typed = new Typed('.yiyan', {
              strings: conte,
              typeSpeed: 30
            });
          }).catch(error=>{
             this.$message({
              message: '网卡未检测到连接',
              type: 'error'
              })
          })
      },
    },
    mounted() {
      this.netWorkCheck();
    }
  }
</script>