<template>
  <div style="height: 100vh;overflow-x: hidden;" class="wrapper">
     <!-- 回到顶部 -->
      <el-backtop target=".wrapper">
        <i class="el-icon-caret-top"></i>
      </el-backtop>
      

    <el-container style="width: 90%;margin-left: 5%">
      <el-header class="test"><Head></Head></el-header>
      <el-container>
        <el-main>
            <div class="blog_bg_left">
              <div class=""></div>
              <div>
                <div class="blog_card" v-for="(item,index) in blogData" :key="index" >
                  <el-row :gutter="20">
                    <el-col :span="17">
                      <div>
                        <router-link :to="'/blog/'+item.id"><p style="color: #303133">{{item.title}}</p></router-link>
                        <div>
                          <el-tag style="margin-right: 10px;" ><span> {{item.flag}}</span></el-tag>
                          <el-tag style="margin-right: 10px;" ><span > {{item.ttype.name}}</span></el-tag>
                          <el-tag>
                            <i style="margin-left: 20px;" class="el-icon-view"><span style="font-size: 18px;"> {{item.views}}</span></i>
                            <i style="margin-left: 20px;" class="el-icon-chat-line-square"><span style="font-size: 18px;"> {{item.commentCount}}</span></i>
                            <i style="margin-left: 20px;" class="el-icon-chat-line-square"><span style="font-size: 18px;"> {{item.commentCount}}</span></i>
                          </el-tag>
                        </div>
                      </div>
                    </el-col>
                    <el-col :span="7">
                      <i style="" ><span style="font-size: 17px;font-style: normal"> {{item.createTime}}</span></i>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                </div>
              </div>

              <div style="margin-left: 2%;padding-bottom: 2%;">
                <!-- 分页 -->
                <el-pagination
                    background
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page.sync="currentPage"
                    :page-size="size"
                    layout="total, prev, pager, next"
                    :total="total">
                </el-pagination>
              </div>
            </div>
        </el-main>
        <el-aside width="20%">
            <div class="grid-content bg-purple blog_bg_right ">
              <!-- 简介 -->
              <el-card shadow="hover" class="box-card">
                <div style="text-align: center;">
                  <img width="120" height="120"  style=" border-radius:50%;" src="../assets/image/avatar.jpeg" />
                  <p style="line-height: 0;">Inwhite</p>
                  <p style="font-size: 15px;">星光一点，点亮前方</p>
                </div>
              </el-card>

              <!-- 分类专栏 -->
              <el-card  shadow="hover" class="box-card" style="margin-top: 20px;">
                <div slot="header" class="clearfix">
                  <span>分类专栏</span>
                  <router-link to="/blogType">
                    <el-button style="float: right; padding: 3px 0" type="text">more <i class="el-icon-d-arrow-right"></i></el-button>
                  </router-link>
                </div>
                <div class="text item" v-for="(item,indexTypes) in types" :key="indexTypes" >
                      <span style="font-size: 14px;"  @click="getType(item.id)">
                          <el-card v-if="item.id > 0" shadow="hover" style="margin-bottom: 5px;">
                            <el-link :underline="false">
                               {{item.name }}
                            </el-link>
                            <span style="float: right;"> {{item.countType}}</span>
                          </el-card>
                      </span>
                  <!-- <div v-if="index!=4" style="background-color: #DCDFE6;height: 1px;margin: 5px 0;"></div> -->
                </div>
              </el-card>

              <!-- 热门文章 -->
              <el-card shadow="hover" class="box-card" style="margin-top: 20px;">
                <div slot="header" class="clearfix">
                  <span>热门文章</span>
                  <el-button style="float: right; padding: 3px 0" type="text">Top5</el-button>
                </div>
                <div class="text item" v-for="(item,index) in HotBlog" :key="index" >
                  <router-link :to="'/blog/'+ item.id">
                    <el-link :underline="false">
                      <span style="font-size: 14px;">
                        <el-tag size="small">{{index + 1}}</el-tag> {{item.title}}
                        <i style="margin-left: 5px;" class="el-icon-view"><span style="font-size: 13px;"> {{item.views}}</span></i>
                      </span>
                    </el-link>
                  </router-link>
                  <div v-if="index!=4" style="background-color: #DCDFE6;height: 1px;margin: 12px 0;"></div>
                </div>
              </el-card>
            </div>
        </el-aside>
      </el-container>
    </el-container>

      <!-- <el-backtop target=".page-component__scroll .el-scrollbar__wrap"></el-backtop> -->

      <Foot style="margin-top: 80px;"></Foot>

  </div>
</template>

<script>
import Head from '@/components/Head'
import Foot from '@/components/Foot'
export default {
  name: 'Home',
  components:{ Head,Foot },
  data () {
   return {
     blogData:[],
     HotBlog:[], //热门文章
     types: [],
     total:0,
     currentPage:1,
     size:5,
     mytype:'',
     myblogTags:[],
     btags:null,
     aIndex: '1',
   }
  },
  mounted() {
    this.initBlog()   //页面加载时调用
    this.initHotBlog()   //页面加载时调用
    this.initAllType()   //页面加载时调用
  },
  methods:{
    // 初始化博客列表数据
    initBlog(){
      const _this = this
      this.axios.get('/blog/vuefindByPage?current=' + this.currentPage + '&size=' + this.size).then(resp=>{
        console.log(resp)
        _this.blogData = resp.data.obj.blogList
        _this.total = resp.data.obj.totals

      })
    },
    //初始化热门文章
    initHotBlog(){
      const _this = this
      this.axios.get('/blog/vuefindHotBlog').then(resp=>{
        console.log(resp)
        _this.HotBlog = resp.data.obj

      })
    },
    //查询所有分类
    initAllType(){
      const _this = this
      this.axios.get('/type/getAllType').then(resp=>{
          console.log(resp)
          _this.types = resp.data.obj
      })
    },
    //获取分类数据
    getType(id){
      console.log(id)
      this.type_id = id
      this.initBlogType()
    },
    //初始化博客数据
    initBlogType(){
      const _this = this
      this.axios.get('/blog/vuefindByPage?current=' + this.currentPage + '&size=' + this.size + '&type_id=' + this.type_id).then(resp=>{
        // console.log(resp)
        _this.blogData = resp.data.obj.blogList
        _this.total = resp.data.obj.blogList.length

      })
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    //分页
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      const _this = this
      this.axios.get('/blog/vuefindByPage?current=' + val + '&size=' + this.size).then(resp=>{
        console.log(resp)
        _this.blogData = resp.data.obj.blogList
        _this.total = resp.data.obj.totals

      })
    }
  }
}
</script>

<style scoped>
  /* 卡片 */
  .blog_card{
    width: 95%;
    margin: 2%;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0);
  }
  /* logo */
  .logo{
    margin-left: 150px;
    font-size: 30px;
    font-family: 华文行楷;
    color: #409eff !important; 
  }
  /* 左侧 */
  .blog_bg_left{
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0);
    width: 100%;
    min-height: 500px;
    border: none;
  }
  /* 右侧 */
  .blog_bg_right{
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0);
    width: 100%;
    min-height: 500px;
  }
  .test{/*固定head*/
    position:sticky;
    top: 0px;
  }
  /* 分割线 */
  /*.fenge{ height:3px; border:none; border-top:3px solid  aliceblue;}*/
</style>