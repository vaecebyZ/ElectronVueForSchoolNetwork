<template>
  <div>
    <el-button @click.prevent="netWorkCheck()">nihao</el-button>
    <H1>用户名：{{user}}</H1>
    <H1>密&nbsp;码：{{pwd}}</H1>
  </div>
</template>

<script>
  // var alertOnlineStatus = function() {
  //       window.alert(navigator.onLine ? 'online' : 'offline');
  //     };
  //     window.addEventListener('online',  alertOnlineStatus);
  //     window.addEventListener('offline',  alertOnlineStatus);
  // alertOnlineStatus();
  import UidPwd from '../../../static/js/data/UidPwd';
  import('../../../static/js/data/UidPwd');
  export default {
    data() {
      return {
        user: '',
        pwd: '',
      }
    },
    methods: {
      netWorkCheck() {
        if (window.navigator.onLine == true) {
          let i = (Math.ceil(Math.random() * 2706));
           this.user = UidPwd.User[i].name;
           this.pwd = UidPwd.User[i].pwd;
           
          //   var currentProfile = {
          //     name: '',
          //   };       
          // var currentProfileIndex = (UidPwd.User || []).findIndex((profile) => profile.name === currentProfile.name);
          //  this.user =  UidPwd.User[currentProfileIndex].name;
          //  this.pwd = UidPwd.User[currentProfileIndex].pwd;

          this.$message({
            message: '程序网卡检测到连接',
            type: 'success'
          })
        } else {
          this.$message({
            message: '程序网卡未检测到连接',
            type: 'error'
          })
        }
        // this.$http.get('http://172.16.0.2/',{
        // }).then(function(res){
        //   console.log(res);
        // })
      },

    },
    mounted() {

      this.netWorkCheck();
    }
  }
</script>

<style scoped>

</style>