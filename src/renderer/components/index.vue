<style>
  html,
  body {
    height: 100%;
    padding: 0px;
    margin: 0px;
  }
</style>

<style scoped>
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    height: 483px;
    width: 200px ! important;
    
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    padding: 0px;
  }
</style>

<template>
  <div>
    <el-container>

      <el-aside>

        <div style="padding-top: 10%;padding-left: 10%;height: 70%;">

          <el-steps :active="active" direction="vertical"  >
            <el-step title="连接测试"></el-step>
            <el-step title="网络测试"></el-step>
            <el-step title="心跳检测中♥。。"></el-step>
          </el-steps>

        </div>

      </el-aside>

      <el-main>


        <div>
          <b style="position: absolute;top:463px;right: 5px;"><span class="yiyan"></span></b>
        </div>

      </el-main>

    </el-container>
  </div>
</template>


<script>
  import UidPwd from '../../../static/js/data/UidPwd';
  import Typed from 'typed.js';

  export default {

    data() {
      return {
        user: '',
        pwd: '',
        isInit: true,
        active : 0
      }
    },

    methods: {

      //第一次测试--入口
      netWorkInit() {

       this.active = 2;

        this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {
          
          //正确处理
          if (this.isInit) {
            this.$notify({
              title: '网络链接正常',
              message: '网络正常,开始冲浪咯o(*￣▽￣*)ブ',
              type: 'success',
              position: 'bottom-left'
            });
          }

          this.isInit = false;

          let data = res.data.data;
          let conte = [];

          conte.push(data.constant + " ---" + data.source)

          this.active = 3

          //屏蔽字符过长的语句
          if (conte[0].length > 33) {

            console.log(conte[0].length);

            //重新获取
            this.netWorkInit();

          } else {

            // this.isReGet = true;
            var typed = new Typed('.yiyan', { //文字展示
              strings: conte,
              typeSpeed: 30
            });
            
            this.onHartBeat(); //心跳

          }

        }).catch(error => {

          //请求失败
          console.log(error);

          this.$message({
            message: '网卡未检测到连接',
            type: 'error'
          })

          //执行登录
          this.loginNet();

        })
      },

      //登录方法
      loginNet() {
        let i = (Math.ceil(Math.random() * 2706)); //选取幸运儿
        this.user = UidPwd.User[i].name;
        this.pwd = UidPwd.User[i].pwd;
        //尝试登录
        this.$http.get('http://172.16.0.2/drcom/login?callback=dr1003&DDDDD=' + UidPwd.User[i].name + '%40cmcc&upass=' +
          UidPwd.User[i].pwd + '&0MKKey=123456&R1=0&R3=0&R6=0&para=00&v6ip=&v=7051', {}).then(res => {
          //返回反序列化
          let recode = JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0]);
          // console.log(JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0])); 
          if (recode.result == 1) {

            this.$notify({
              title: '成功接入',
              message: '登上了倒霉蛋的号,开始冲浪咯o(*￣▽￣*)ブ',
              type: 'success',
              position: 'bottom-left'
            });

            this.onHartBeat();

          } else {

            this.loginNet();

          }
        }).catch(error => {

          this.$message({
            message: '网卡未检测到连接',
            type: 'error'
          })

          console.log(error);

        });
      },

      //心跳检测
      onHartBeat() {
        setTimeout(() => {
          this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {
            this.$message({
              message: '网卡检测到连接',
              type: 'success'
            })
            this.onHartBeat();
          }).catch(error => {

            this.$message({
              message: '网卡未检测到连接',
              type: 'error'
            })

            this.loginNet()

          });
        }, 4000);
      }
    },
    mounted() {
       this.$message({
        message: "网络测试中",
        type: "warning"
      })
      this.active = 1;
      setTimeout(() => {
         this.netWorkInit();
      }, 1500);
     

     
    }
  }
</script>