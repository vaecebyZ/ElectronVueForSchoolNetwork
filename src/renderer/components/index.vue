<style>
  html,
  body {
    height: 100%;
    padding: 0px;
    margin: 0px;
  }
</style>

<style scoped>
  #leftButton {
    width: 200px !important;
    color: rgb(80, 255, 27);
    height: 144px;
    background-color: black;
    margin: 0px;
    padding: 0px;
  }


  #topleft {
    width: 200px !important;
    color: #333;
    height: 543px;
    background-color: #D3DCE6;

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

      <el-aside id="topleft">

        <div style="padding-top: 10%;padding-left: 10%;height: 70%;">

          <el-steps :active="active" direction="vertical" finish-status="success">
            <el-step title="连接测试"></el-step>
            <el-step title="网络测试"></el-step>
            <el-step title="获取账号中"></el-step>
            <el-step title="心跳检测中♥..."></el-step>
          </el-steps>

        </div>

        <el-aside id="leftButton">
          <div>
            <!-- v-infinite-scroll="load" -->
            <ul id="teminial" style="overflow:auto;list-style: none; margin: 0px;padding-left: 20px;">
              <li v-for="(item,index) in count" :key="index"><span
                  style=" font-size:12px ;">{{count[index].time}}➡{{count[index].title}}</span></li>
            </ul>

          </div>
          <el-drawer title="我是标题" :visible.sync="drawer" :direction="direction" :with-header="false">
            <span>我来啦!</span>
          </el-drawer>
        </el-aside>


      </el-aside>

      <el-main>

        <div>
          <b style="position: absolute;top:503px;right: 5px;"><span class="yiyan"></span></b>
        </div>
        <el-button @click="drawer = true" type="primary">清除消息</el-button>
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
        active: 0,
        count: [],
        drawer: false,
        direction: 'ltr',
      }
    },
    watch: {
      //实现数据更新后锁定最后一条数据
      count: 'scrollToBottom'
    },
    methods: {



      //锁定方法
      scrollToBottom() {

        this.$nextTick(() => {

          var div = document.getElementById('leftButton')

          div.scrollTop = div.scrollHeight

        })

      },
      //获取时间
      msgPush(msgs) {

        let d = new Date();

        let hour = d.getHours(); //得到小时数

        let minute = d.getMinutes(); //得到分钟数

        let second = d.getSeconds(); //得到秒数

        let msg = {
          time: hour + ":" + minute + ":" + second,
          title: msgs
        }

        this.count.push(msg)

      },
      addOne() {
        this.count.push('233');
        let msg = document.getElementById('teminial') // 获取对象
        msg.scrollTop = msg.scrollHeight // 滚动高度
      },

      //第一次测试--入口
      netWorkInit() {

        this.active++;

        this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {


          //正确处理
          if (this.isInit) {
            this.msgPush('网络连接正常✔');
            this.$notify({
              title: '网络链接正常✔',
              message: '网络正常,开始冲浪咯o(*￣▽￣*)ブ',
              type: 'success',
              //position: 'bottom-left'
            });
          }

          this.isInit = false;

          let data = res.data.data;
          let conte = [];

          conte.push(data.constant + " ---" + data.source)
          this.active += 2;

          //屏蔽字符过长的语句
          if (conte[0].length > 39) {

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

          this.active++;

          this.$message({
            message: '网卡未检测到连接❌',
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
        this.msgPush('尝试:' + this.user);
        this.$http.get('http://172.16.0.2/drcom/login?callback=dr1003&DDDDD=' + UidPwd.User[i].name + '%40cmcc&upass=' +
          UidPwd.User[i].pwd + '&0MKKey=123456&R1=0&R3=0&R6=0&para=00&v6ip=&v=7051', {}).then(res => {
          //返回反序列化
          let recode = JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0]);
          // console.log(JSON.parse(res.data.match(/[^\(\)]+(?=\))/g)[0]));
          if (recode.result == 1) {
            this.msgPush('成功接入✔');
            this.$notify({
              title: '成功接入✔',
              message: '登上了正确的号,开始冲浪咯o(*￣▽￣*)ブ',
              type: 'success',
              // position: 'bottom-left'
            });

            this.active++;

            this.onHartBeat();

          } else {
            this.msgPush('账号问题切账号❗');
            this.loginNet();

          }
        }).catch(error => {

          this.$message({
            message: '网卡未检测到连接❌',
            type: 'error'
          })

          // console.log(error);

        });
      },

      //心跳检测
      onHartBeat() {
        setTimeout(() => {
          this.$http.get('https://api.muxiaoguo.cn/api/yiyan', {}).then(res => {
            // this.$message({
            //   message: '网卡检测到连接',
            //   type: 'success'
            // })




            this.msgPush('心跳测试❤');
            //console.log(msg.time);
            // this.count.push(msg);

            this.onHartBeat();
          }).catch(error => {

            this.$message({
              message: '网卡未检测到连接❌',
              type: 'error'
            })

            this.loginNet()

          });
        }, 4000);
      }
    },
    mounted() {

      // this.$message({
      //   message: "网络测试中",
      //   type: "warning"
      // })

      this.msgPush('网络测试中...');

      setTimeout(() => {
        this.active++;
        this.netWorkInit();
      }, 1500);



    }
  }
</script>