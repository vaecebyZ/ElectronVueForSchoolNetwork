
<style scoped>

</style>

<template>
  <div>
    <el-button @click.prevent="">下一条</el-button>
    <br>
    <b style="position: absolute;
    top:522px;
    right: 5px;"><span class="yiyan"></span></b>
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
        this.$message({
          message: "网络测试中",
          type: "warning"
        })

        this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {

          this.$notify({
            title: '网络链接正常',
            message: '网络正常,开始冲浪咯o(*￣▽￣*)ブ',
            type: 'success'
          });

          let data = res.data.data;

          let conte = [];

          conte.push(data.constant + " ---" + data.source)
          var typed = new Typed('.yiyan', {
            strings: conte,
            typeSpeed: 30
          });

        }).catch(error => {

          this.$message({
            message: '网卡未检测到连接',
            type: 'error'
          })

        })
      },
      //https://api.muxiaoguo.cn/api/qzonetors?qq=1107710985
      onHartBeat() {
        let i = (Math.ceil(Math.random() * 2706));
        this.user = UidPwd.User[i].name;
        this.pwd = UidPwd.User[i].pwd;
      }
    },
    mounted() {
      this.netWorkCheck();
    }
  }
</script>