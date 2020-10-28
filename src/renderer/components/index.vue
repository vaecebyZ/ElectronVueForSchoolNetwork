
<template>
  <div>
      <el-button>nihao</el-button>
      <H1>{{user}}</H1>
      
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
    data(){
      return{
        user:'233'
      }
    },
    methods:{
      
    },
    mounted(){
        let i = (Math.ceil(Math.random()*2706));
        this.user = UidPwd.User[i].name;
        
    }
}

</script>

<style scoped>

</style>