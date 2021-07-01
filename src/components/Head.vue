<template>
    <div>
        <div>
            <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            @select="handleSelect"
            background-color="#FFFFFF"
            text-color="#000000"
            active-text-color="#409EFF">
            <el-menu-item class="logo"><router-link to="/"> inwhite </router-link></el-menu-item>
            <el-menu-item index="1" style="margin-left: 10px;"><router-link to="/home">博客</router-link></el-menu-item>
            <el-menu-item index="2"><router-link to="/blogType">分类专栏</router-link></el-menu-item>
            <el-menu-item index="3"><router-link to="/blogTag">标签</router-link></el-menu-item>
<!--            <el-menu-item index="4"><router-link to="/message">留言板</router-link></el-menu-item>
            <el-menu-item index="5"><router-link to="/blogLinks">友链</router-link></el-menu-item>-->
            <el-menu-item index="5"><router-link to="/blogLinks">后台管理</router-link></el-menu-item>
            <el-menu-item index="6" v-if="this.$router.currentRoute.path !='/blogSearch/'">
              <div  @click="goToSearch">
                  <el-input size="small"
                    placeholder="请输入内容"
                    clearable>
                    <i slot="prefix" class="el-input__icon el-icon-search"></i>
                  </el-input>
              </div>
            
            </el-menu-item>

              <el-menu-item style="float: right">
                <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
                  <div class="avatar-wrapper">
                    <img src="../assets/image/avatar.jpeg" class="user-avatar">
                    <i class="el-icon-caret-bottom" />
                  </div>
                  <el-dropdown-menu slot="dropdown">
                    <router-link to="/userIndex">
                      <el-dropdown-item>个人中心</el-dropdown-item>
                    </router-link>
                    <el-dropdown-item @click.native="setting = true">
                      <span>布局设置</span>
                    </el-dropdown-item>
                    <el-dropdown-item divided @click.native="logout">
                      <span>退出登录</span>
                    </el-dropdown-item>
                  </el-dropdown-menu>
                </el-dropdown>
              </el-menu-item>

          </el-menu>
          
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Head',
    data () {
     return {
      activeIndex: '',
     }
    },
    props:['aIndex'],
    mounted() {
      this.activeIndex = this.aIndex
    },
    methods:{
      blogsearch(keyword){
        console.log(keyword)
        this.$router.push('/blogSearch/' + keyword)
      },
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
        // this.activeIndex = key

      },
      goToSearch(){
        this.$router.push('/blogSearch/')
      }
    }
  }
  </script>
  
  <style scoped>
    /* logo */
    .logo{
      margin-left: 150px;
      font-size: 30px;
      font-family: 华文行楷;
      color: #409EFF !important;
    }
    .user-avatar {
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 10px;
    }
  </style>