<template>
  <div>
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-setting"></i> 概况</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="plugins-tips">
      <div class="content-title">共
        <span class="number cursor" @click="jumpToArticleManage">{{1}}</span>名注册用户，贡献了
        <span class="number">{{2}}</span>个问题，
        <span class="number">{{3}}</span>名用户正在解决问题。</div>
    </div>
    <el-row :gutter="10">
      <el-col :xs="24" :sm="24" :md="8" :lg="8" :xl="8">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>登陆信息</span>
          </div>
          <div class="login-info">
              <p class="avatar"><img src="http://p6a87gauo.bkt.clouddn.com/user_23.png"></p>
              <p class="name">iimT</p>
              <p class="identify">管理员</p>
              <p class="time">2018.4.10 21:29:08</p>
          </div>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="24" :md="8" :lg="8" :xl="8">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>待办事项</span>
          </div>
          <div class="text item">
              <el-badge :value="12" class="item">
                <el-button size="small">待审核问题</el-button>
              </el-badge>
          </div>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="24" :md="8" :lg="8" :xl="8">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>公告</span>
          </div>
          <div class="text item">
              <p>这里是公告：目前题目较少，请多贡献题目。积极审核题目。</p>
          </div>
        </el-card>
      </el-col>
      <!-- 整列 -->
      <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>日活跃</span>
          </div>
          <div class="text item">
              <schart canvasId="line" width="1000" height="300" :data="dailyQpassedData" type="line" :options="dailyQpassedOptions"></schart>
          </div>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>每日答题量</span>
          </div>
          <div class="text item">
              <schart canvasId="bar" width="1000" height="300" :data="dailyQpassedData" type="bar" :options="dailyQpassedOptions"></schart>
          </div>
        </el-card>
      </el-col>

      
      
    </el-row>
    <div class="ms-doc">
      <h3>README.md</h3>
      <article>
        <h1>T-Blog</h1>
        <p>基于Vue + Element + PhalAPI的博客后台</p>
        <h2>前言</h2>
        <p>对之前Shell风格的后台重新做的一套可视化面板。</p>
        <h2>TODO:</h2>
        <el-checkbox disabled checked>评论管理</el-checkbox>
        <br>
        <el-checkbox disabled checked>创作时的实时保存</el-checkbox>
        <br>
        <el-checkbox disabled checked>前台博文中的评论升级为批注样式</el-checkbox>
        <br>
      </article>
    </div>
  </div>
</template>

<script>
import Schart from "vue-schart";
export default {
  data: function() {
    return {
            dailyQpassedData:[
                {name:'4.1',value:1141},
                {name:'4.2',value:1499},
                {name:'4.3',value:2260},
                {name:'4.4',value:1170},
                {name:'4.5',value:970},
                {name:'4.6',value:1450},
                {name:'4.7',value:2260},
                {name:'4.8',value:1170},
                {name:'4.9',value:970},
                {name:'4.10',value:1450}
            ],
            data2 : [
                {name:'短袖',value:1200},
                {name:'休闲裤',value:1222},
                {name:'连衣裙',value:1283},
                {name:'外套',value:1314},
                {name:'羽绒服',value:2314}
            ],
            options1: {
                title: '某商店近年营业总额',
                bgColor: '#009688',
                titleColor: '#ffffff',
                fillColor: '#e0f2f1',
                axisColor: '#ffffff',
                contentColor: '#999'
            },
            dailyQpassedOptions: {
                bgColor: '#fff',
                titleColor: '#ffffff',
                legendColor: '#ffffff'
            }
        };
  },
  methods: {
    jumpToArticleManage() {
      this.$router.push("/article");
    }
  },
  components: {
    Schart
  },
}
</script>

<style scoped>
.login-info p{
  text-align: center;
  margin: 10px 0;
}
.login-info p.avatar img {
  margin-top: -15px;
  height: 60px;
  border-radius: 5px;
}
.login-info p.name {
  font-size: 18px;
  font-weight: bold;
  border-radius: 5px;
}
.box-card {
  margin-bottom: 1rem;
}
.box-card .el-card__body {
  padding: 0;
}
.number {
  color: #ff6655;
  font-size: 50px;
  padding: 0 8px;
}
.cursor {
  cursor: pointer;
}
.content-title {
  padding: 0 30px;
  font-weight: 400;
  line-height: 50px;
  margin: 10px 0;
  font-size: 24px;
  color: #1f2f3d;
}
.ms-doc {
  width: 100%;
  max-width: 980px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial,
    sans-serif;
}
.ms-doc h3 {
  padding: 9px 10px 10px;
  margin: 0;
  font-size: 14px;
  line-height: 17px;
  background-color: #f5f5f5;
  border: 1px solid #d8d8d8;
  border-bottom: 0;
  border-radius: 3px 3px 0 0;
}
.ms-doc article {
  padding: 45px;
  word-wrap: break-word;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-right-radius: 3px;
  border-bottom-left-radius: 3px;
}
.ms-doc article h1 {
  font-size: 32px;
  padding-bottom: 10px;
  margin-bottom: 15px;
  border-bottom: 1px solid #ddd;
}
.ms-doc article h2 {
  margin: 24px 0 16px;
  font-weight: 600;
  line-height: 1.25;
  padding-bottom: 7px;
  font-size: 24px;
  border-bottom: 1px solid #eee;
}
.ms-doc article p {
  margin-bottom: 15px;
  line-height: 1.5;
}
.ms-doc article .el-checkbox {
  margin-bottom: 5px;
}
</style>