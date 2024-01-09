<template>
  <el-aside :style="`width:${asideStyle}px;height:100%;`">
    <el-menu 
      :default-active="bMenuIndex"
      class="el-menu-vertical-demo"
      background-color="#556D84"
      text-color="#fff"
      :collapse="bAsideStatus"
      :show-timeout="300"
    >
      <router-link to='/'>
        <el-menu-item index="1">
          <i class="el-icon-s-home"></i>
          
          <span slot="title">酒店经理首页</span>
        </el-menu-item>
      </router-link>

      <router-link to='/checkIn'>
        <el-menu-item index="2">
          <i class="el-icon-s-custom"></i>
          <span slot="title">前台登记页面</span>
        </el-menu-item>
      </router-link>

     <router-link to='/admintest'>
        <el-menu-item index="3">
          <i class="el-icon-s-operation"></i>
          <span slot="title">空调管理页面</span>
        </el-menu-item>
      </router-link>

      <!-- <router-link to='/history'>
        <el-menu-item index="3">
          <i class="el-icon-menu"></i>
          <span slot="title">历史纪录查询</span>
        </el-menu-item>
      </router-link> -->

      <!-- <router-link to='/statistics'>
        <el-menu-item index="4">
          <i class="el-icon-s-data"></i>
          <span slot="title">查看收入统计</span>
        </el-menu-item>
      </router-link> -->

      <!-- <router-link to='/roomInfor'>
        <el-menu-item index="5">
          <i class="el-icon-setting"></i>
          <span slot="title">房间信息管理</span>
        </el-menu-item>
      </router-link> -->

      <!-- <router-link to='/control'>
        <el-menu-item index="6">
          <i class="el-icon-s-tools"></i>
          <span slot="title">空调控制面板</span>
        </el-menu-item>
      </router-link> -->

      
    </el-menu>
  </el-aside>
</template>


<script>

import bus from '../assets/bus.js';
export default {
  data(){
    return {
      bMenuIndex:'1',
      bAsideStatus:false,
      asideStyle:240
    }
  },
  mounted(){
    let router=this.$route.path;
    switch(router){
      case '/':
        this.bMenuIndex='1';
      break;
      case '/checkIn':
        this.bMenuIndex='2';
      break;
      case '/admintest':
        this.bMenuIndex='3';
      break;
      // case '/statistics':
      //   this.bMenuIndex='4';
      // break;
      // case '/roomInfor':
      //   this.bMenuIndex='5';
      // break;
      // case '/control':
      //   this.bMenuIndex='6';
      // break;

      break;
      default:
        this.bMenuIndex='1';
    }
    this.bAsideStatus=window.localStorage.getItem('asideStatus');
    // console.log(this.bAsideStatus)
    if(this.bAsideStatus===null){
      this.bAsideStatus=false;
    }else{
      // this.bAsideStatus=true;
      this.bAsideStatus=this.bAsideStatus==='false'?false:true;
    }
    console.log(this.b)
    if(this.bAsideStatus){
      this.asideStyle=65;
    }else{
      this.asideStyle=240;
    }
    bus.$on('changeStatus',(data)=>{
      this.bAsideStatus=data;
      if(this.bAsideStatus){
        this.asideStyle=65;
      }else{
        this.asideStyle=240;
      }
    })
  },
  methods: {
    
  }
}
</script>
