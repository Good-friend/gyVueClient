<template>
  <div>

    <div class="fly-panel fly-column">
      <div class="layui-container">
        <ul class="layui-clear">
          <li class="layui-hide-xs layui-this"><a href="/">最新<span class="layui-badge-dot"></span></a></li>
          <li><a href="javascript:;" @click="queryType($event,'question')">问题</a></li>
          <li><a href="javascript:;" @click="queryType($event,'gambit')">话题</a></li>
          <li><a href="javascript:;" @click="queryType($event,'share')">分享</a></li>
          <li><a href="javascript:;" @click="queryType($event,'original')">原创</a></li>
          <li class="layui-hide-xs layui-hide-sm layui-show-md-inline-block"><span class="fly-mid"></span></li>

          <!-- 用户登入后显示 -->
          <li v-if='userSession != null' class="layui-hide-xs layui-hide-sm layui-show-md-inline-block"><a href="javascript:;" @click="myPosting()">我的发表</a></li>
          <li v-if='userSession != null' class="layui-hide-xs layui-hide-sm layui-show-md-inline-block"><a href="javascript:;" @click="myCollection()">我的收藏</a></li>
        </ul>

        <div v-if='userSession != null' class="fly-column-right layui-hide-xs">
          <!--<span class="fly-search"><i class="layui-icon"></i></span>-->
          <a href="javascript:;" @click="add()" class="layui-btn">发表新帖</a>
        </div>
        <div v-if='userSession != null' class="layui-hide-sm layui-show-xs-block" style="margin-top: -10px; padding-bottom: 10px; text-align: center;">
          <a href="javascript:;" @click="add()" class="layui-btn">发表新帖</a>
        </div>
      </div>
    </div>

    <div class="layui-container">
      <div class="layui-row layui-col-space15">
        <div class="layui-col-md8">
          <div class="fly-panel">
            <div class="fly-panel-title fly-filter">
              <h3>置顶</h3>
              <!--<a href="#signin" class="layui-hide-sm layui-show-xs-block fly-right" id="LAY_goSignin" style="color: #FF5722;">去签到</a>-->
            </div>
            <ul class="fly-list">
              <li v-for="(item ,i) in stickList" :key="i" v-if="item.stick=='1'">
                <a href="javascript:;" class="fly-avatar" @click="authorInfo(item.author)">
                  <img :src="item.headPortraitUrl" alt="葛耀">
                </a>
                <h2>
                  <a class="layui-badge">{{item.typeName}}</a>
                  <a href="javascript:;" @click="detail(item.serialNumber)">{{item.title}}</a>
                </h2>
                <div class="fly-list-info">
                  <a href="javascript:;" @click="authorInfo(item.author)">
                    <cite>{{item.nickName}}</cite>
                    <i class="iconfont icon-renzheng" title="认证信息：XXX"></i>
                    <!--<i class="layui-badge fly-badge-vip">VIP3</i>-->
                  </a>
                  <span>{{item.createTime}}</span>

                 <!-- <span class="fly-list-kiss layui-hide-xs" title="悬赏飞吻"><i class="iconfont icon-kiss"></i> 60</span>-->
                  <span class="layui-badge fly-badge-accept layui-hide-xs" v-if="item.status =='1'">已结</span>
                  <span class="fly-list-nums">
                <i class="iconfont icon-pinglun1" title="回答"></i>{{item.commentTimes}}
              </span>
                </div>
                <div class="fly-list-badge">
                  <span class="layui-badge layui-bg-black">置顶</span>
                  <span class="layui-badge layui-bg-red" v-if="item.best =='1'">精帖</span>
                </div>
              </li>
            </ul>
          </div>

          <div class="fly-panel" style="margin-bottom: 0;">

            <div class="fly-panel-title fly-filter">
              <span id="select1">
                <a href="javascript:;" class="layui-this" @click="query($event,0)">综合</a>
                <span class="fly-mid"></span>
                <a href="javascript:;" @click="query($event,1)">未结</a>
                <span class="fly-mid"></span>
                <a href="javascript:;" @click="query($event,2)">已结</a>
                <span class="fly-mid"></span>
                <a href="javascript:;" @click="query($event,3)">精华</a>
              </span>
              <span class="fly-filter-right layui-hide-xs">
                <a href="javascript:;" class="layui-this" @click="query($event,4)">按最新</a>
                <span class="fly-mid"></span>
                <a href="javascript:;" @click="query($event,5)">按热议</a>
              </span>
            </div>

            <ul class="fly-list">
              <li v-for="(item ,i) in catalogueList" :key="i" v-if="item.stick=='0'">
                <a href="javascript:;" class="fly-avatar" @click="authorInfo(item.author)">
                  <img :src="item.headPortraitUrl" alt="">
                </a>
                <h2>
                  <a class="layui-badge">{{item.typeName}}</a>
                  <a href="javascript:;" @click="detail(item.serialNumber)">{{item.title}}</a>
                </h2>
                <div class="fly-list-info">
                  <a href="javascript:;" @click="authorInfo(item.author)">
                    <cite>{{item.nickName}}</cite>
                    <i class="iconfont icon-renzheng" title="认证信息：XXX"></i>
                    <!--

                    <i class="layui-badge fly-badge-vip">VIP3</i>
                    -->
                  </a>
                  <span>{{item.createTime}}</span>

                  <!--<span class="fly-list-kiss layui-hide-xs" title="悬赏飞吻"><i class="iconfont icon-kiss"></i> 60</span>-->
                  <span class="layui-badge fly-badge-accept layui-hide-xs" v-if="item.status =='1'">已结</span>
                  <span class="fly-list-nums">
                <i class="iconfont icon-pinglun1" title="回答"></i>{{item.commentTimes}}
              </span>
                </div>
                <div class="fly-list-badge" v-if="item.best =='1'">
                  <span class="layui-badge layui-bg-red">精帖</span>
                </div>
              </li>
            </ul>
            <div style="text-align: center">
              <div class="laypage-main">
                <a href="javascript:;" class="laypage-next" @click="moreData()" v-show="catalogueList.length > 14">查看更多</a>
              </div>
            </div>

          </div>
        </div>
        <div class="layui-col-md4">
          <div class="fly-panel fly-signin">
            <div class="fly-panel-title">
              小站说明
             <!-- <i class="fly-mid"></i>
              <a href="javascript:;" class="fly-link" id="LAY_signinHelp">说明</a>
              <i class="fly-mid"></i>
              <a href="javascript:;" class="fly-link" id="LAY_signinTop">活跃榜<span class="layui-badge-dot"></span></a>
              <span class="fly-signin-days">已连续签到<cite>16</cite>天</span>-->
            </div>
            <div class="fly-panel-main first_explain">
              <p>欢迎光临小站，希望您能在小站里找到你想要的东西。该小站暂时不支持注册。如果你也想成为小站会员，请去留言去留下您的邮箱，小站将给你免费发送会员信息</p>

              <!--<button class="layui-btn layui-btn-danger" id="LAY_signin">今日签到</button>
              <span>可获得<cite>5</cite>飞吻</span>-->

              <!-- 已签到状态 -->
              <!--
              <button class="layui-btn layui-btn-disabled">今日已签到</button>
              <span>获得了<cite>20</cite>飞吻</span>
              -->
            </div>
          </div>
          <div class="fly-panel">
            <h3 class="fly-panel-title">友情连接</h3>
            <ul class="fly-panel-main fly-list-static">
              <li>
                <a href="http://www.rongxintong.com" target="_blank">融信通金融服务平台</a>
              </li>
              <li>
                <a href="http://jiaofei.rongxintong.com" target="_blank">融信通云缴费平台（龙E达）</a>
              </li>
              <li>
                <a href="https://app.rongxintong.com:1448" target="_blank">融信通收银台</a>
              </li>
              <li>
                <a href="https://app.rongxintong.com:1449" target="_blank">融信通云租赁平台</a>
              </li>
            </ul>
          </div>

          <div class="fly-panel fly-rank fly-rank-reply" id="LAY_replyRank">
            <h3 class="fly-panel-title">发帖榜</h3>
            <dl>
              <!--<i class="layui-icon fly-loading">&#xe63d;</i>-->
              <dd v-for="(item ,i) in countByAuthor" :key="i">
                <a href="javascript:;" @click="authorInfo(item.username)">
                  <img :src="item.head_portrait_url"><cite>{{item.nickname}}</cite><i>{{item.cataCount}}次发帖</i>
                </a>
              </dd>
            </dl>
          </div>

          <!--<dl class="fly-panel fly-list-one">
            <dt class="fly-panel-title">热门帖</dt>
            <dd>
              <a href="jie/detail.html">基于 layui 的极简社区页面模版</a>
              <span><i class="iconfont icon-pinglun1"></i> 16</span>
            </dd>
            <div class="fly-none">没有相关数据</div>
          </dl>-->

          <!--<div class="fly-panel">
            <div class="fly-panel-title">
              这里可作为广告区域
            </div>
            <div class="fly-panel-main">
              <a href="http://layim.layui.com/?from=fly" target="_blank" class="fly-zanzhu" time-limit="2017.09.25-2099.01.01" style="background-color: #5FB878;">LayIM 3.0 - layui 旗舰之作</a>
            </div>
          </div>-->

          <div class="fly-panel fly-link">
            <h3 class="fly-panel-title">小站技术点</h3>
            <dl class="fly-panel-main">
              <dd><a href="https://cn.vuejs.org/" target="_blank">Vue</a></dd>
              <dd><a href="https://www.layui.com/" target="_blank">layui</a></dd>
              <dd><a href="https://spring.io/projects/spring-boot" target="_blank">Springboot</a></dd>
            </dl>
          </div>

        </div>
      </div>
    </div>


  </div>

</template>

<script>
  export default {
      name: "first-page",
      data: function() {
        return{
          userSession:JSON.parse(sessionStorage.getItem('user')),
          queryUrl:"lobby/queryCatalogueList?",
          catalogueList:[],
          stickList:[],
          queryParams:{isEnd:'',orderType:'id',isbest:''},
          countByAuthor:[],
          type:''
        }
      },
      methods:{
        myCollection:function(){
          this.$router.push({path: 'myCollection'})
        },
        myPosting:function(){
          this.$router.push({path: 'myPosting'})
        },
        add:function(){
          this.$router.push({path: 'newEdit'})
        },
        detail:function(id){
          this.$router.push({path: 'detail',query:{id:id}})
        },
        query:function(e,src){
          var dom = $(e.srcElement);
          if(!dom.hasClass('layui-this')){
            dom.parent().find('.layui-this').removeClass('layui-this');
            dom.addClass('layui-this');
          }
          var url = this.queryUrl+'stick=0'
          switch (src){
            case 0://综合
              this.queryParams.isEnd =''
              this.queryParams.isbest = ''
              break;
            case 1://未结
              this.queryParams.isEnd ='0'
              this.queryParams.isbest = ''
              break;
            case 2://已结
              this.queryParams.isEnd ='1'
              this.queryParams.isbest = ''
              break;
            case 3://精华
              this.queryParams.isEnd =''
              this.queryParams.isbest = '1'
              break;
            case 4://最新
              this.queryParams.orderType = 'id'
              break;
            case 5://热议
              this.queryParams.orderType = 'heat'
              break;

          }
          console.info(this.queryParams)
          url += '&orderType='+this.queryParams.orderType

          if(this.queryParams.isEnd){
            url += '&status='+this.queryParams.isEnd
          }
          if(this.queryParams.isbest){
            url += '&best='+this.queryParams.isbest
          }
          this.$http.get(url,layer,this).then(result => {
            this.catalogueList = result.data.catalogueList
          });
        },
        authorInfo:function (username) {
          //this.$router.push({path: 'myHomepage',query:{nickname:username}})
          this.$router.push({name: 'my-homepage', params: {username: username}})
        },
        queryType:function (e,type) {
          var dom = $(e.srcElement).parent();
          if(!dom.hasClass('layui-this')){
            dom.parent().find('.layui-this').removeClass('layui-this');
            dom.addClass('layui-this');
          }
          this.type = type
          this.$http.get(this.queryUrl+'type='+type,layer,this).then(result => {
            this.catalogueList = result.data.catalogueList
            this.stickList = result.data.stickList
          });
        },
        moreData:function () {
          var url = this.queryUrl+'stick=0&orderType='+this.queryParams.orderType
          if(this.queryParams.isEnd){
            url += '&status='+this.queryParams.isEnd
          }
          if(this.queryParams.isbest){
            url += '&best='+this.queryParams.isbest
          }
          if(this.type){
            url += '&type'+this.type
          }
          var start = this.catalogueList.length + 1
          var end = this.catalogueList.length + 15
          var queryCountStr = start+','+end
          this.$http.get(url+"&queryCountStr="+queryCountStr,layer,this).then(result => {
            this.catalogueList = this.catalogueList.concat(result.data.catalogueList)
            if(result.data.catalogueList.length < 15){
              $('.laypage-next').hide()
            }
          })
        }
      },
      mounted() {


      },
      created() {
        let _this = this
        layui.use('layer', function() {
          layer = layui.layer;
          var type = _this.$route.params.type
          var url = _this.queryUrl + 'orderType=id'
          if (type) {
            url += '&type=' + type;
          }
          _this.$http.get(url,layer,_this).then(result => {
            if(result && result.data){
              _this.catalogueList = result.data.catalogueList
              _this.stickList = result.data.stickList
              _this.countByAuthor = result.data.countByAuthor
            }
          })
        })
      }
  }
</script>

<style scoped>

  .first_explain p{
    font-size: 14px;
    line-height: 26px;
    color: #666;
  }
  .fly-panel-title{
    color: #000000;
  }
  .layui-col-md8 ul li:hover{
    /*border: 2px solid #5FB878;*/
    box-shadow: 0 0 10px rgba(95,184,120,0.3);
    /*box-shadow: 0 0 10px rgba(1,170,237,0.6);*/
  }
</style>
