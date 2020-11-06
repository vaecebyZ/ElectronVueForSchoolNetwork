<style>
  html,
  body {
    height: 100%;
    padding: 0px;
    margin: 0px;
  }


  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    height: 543px;
    /* text-align: center; */
    /* height: 100%; */
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    /* text-align: center; */
    /* height: 100%; */
  }
</style>

<template>
  <div>
    <el-container>
      <el-aside>Aside</el-aside>
      <el-main>
        <!-- <b style="position: absolute;
    top:522px;
    right: 5px;"><span class="yiyan"></span></b> -->
        <div><b><span style="position: absolute;
    top:522px;
    right: 5px;" class="yiyan"></span></b></div>
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
        pwd: '123',
        isInit: true,
        isReGet:false
      }
    },

    methods: {

      //第一次测试
      netWorkInit() {
       
        this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => { //正确处理

          this.inItFunc(res.data.data);

        }).catch(error => {

          console.log(error);

          this.$message({
            message: '网卡未检测到连接',
            type: 'error'
          })

          this.loginNet();

        })
      },
      inItFunc(resdata) {

        if (this.isInit) {
          this.$notify({
            title: '网络链接正常',
            message: '网络正常,开始冲浪咯o(*￣▽￣*)ブ',
            type: 'success',
            position: 'bottom-left'
          });
        }

        this.isInit = false;

        let data = resdata;
        let conte = [];

        conte.push(data.constant + " ---" + data.source)


        if (conte[0].length > 33) {
          console.log(conte[0].length);
          this.netWorkInit();

        } else {
          this.isReGet = true;
          var typed = new Typed('.yiyan', { //文字展示
            strings: conte,
            typeSpeed: 30
          });
        }
          if(this.isReGet){
              this.onHartBeat(); //心跳
            }
        
      },

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

      },
      loginNet() {
        let i = (Math.ceil(Math.random() * 2706)); //选取幸运儿
        this.user = UidPwd.User[i].name;
        this.pwd = UidPwd.User[i].pwd;
        //尝试登录
        this.$http.get('http://172.16.0.2/drcom/login?callback=dr1003&DDDDD=' + UidPwd.User[i].name + '%40cmcc&upass=' +
          UidPwd.User[i].pwd + '&0MKKey=123456&R1=0&R3=0&R6=0&para=00&v6ip=&v=7051', {}).then(res => {
          let recode = JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0]);
          console.log(JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0])); //返回反序列化
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
      }
    },
    mounted() {
       this.$message({
          message: "网络测试中",
          type: "warning"
          })
      this.netWorkInit();
    }
  }
</script>