<template>
  <el-header>
    <div class="header">
      <div class="header-left">
        
        <!-- <img src="../assets/images/logo.png" alt="logo" @click="changeAside"> -->
        <div class= "menu_click"    @click="changeAside ">
          <i class="icon-menu" style="" ></i>
          <a class="header-homepage" @click = "goHome">酒店管理系统</a>

        </div>
        
        <!-- <i class="icon-menu" style="" @click="changeAside"></i>
        <a class="header-homepage">酒店管理系统</a> -->
        
        

          <!-- <svg version="1.1" role="presentation" width="22.857142857142854" height="22.857142857142854"  viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" class="svg-icon2"> -->
          <!-- <path fill="currentColor" d="M389.44 768a96.064 96.064 0 0 1 181.12 0H896v64H570.56a96.064 96.064 0 0 1-181.12 0H128v-64h261.44zm192-288a96.064 96.064 0 0 1 181.12 0H896v64H762.56a96.064 96.064 0 0 1-181.12 0H128v-64h453.44zm-320-288a96.064 96.064 0 0 1 181.12 0H896v64H442.56a96.064 96.064 0 0 1-181.12 0H128v-64h133.44z"></path></svg> -->

        <!-- <i class="el-icon-s-fold" style="" @click="changeAside"></i> -->
        
      </div>

      <div class="header-middle"  @click = "goHome">
        <img src="../assets/images/hotel_logo.png" alt="logo">
      </div>

      <div class="header-right">
        <div class="svg-icon-div" @click="fFullScreen">
          <svg version="1.1" role="presentation" width="22.857142857142854" height="22.857142857142854" viewBox="0 0 1024 1024" class="svg-icon">
            <path d="M868.6 96H720c-7.6 0-15.2 2.9-21 8.7-5.8 5.8-8.7 13.4-8.7 21s2.9 15.2 8.7 21c5.8 5.8 13.4 8.7 21 8.7h106.5L580.1 401.8c-11.6 11.6-11.6 30.4 0 42 11.6 11.6 30.4 11.6 42 0l246.4-246.4V304c0 7.6 2.9 15.2 8.7 21 5.8 5.8 13.4 8.7 21 8.7s15.2-2.9 21-8.7c5.8-5.8 8.7-13.4 8.7-21V155.4c0.1-32.8-26.5-59.4-59.3-59.4zM898.3 690.3c-7.6 0-15.2 2.9-21 8.7-5.8 5.8-8.7 13.4-8.7 21v106.5L622.2 580.1c-11.6-11.6-30.4-11.6-42 0s-11.6 30.4 0 42l246.4 246.4H720c-7.6 0-15.2 2.9-21 8.7-5.8 5.8-8.7 13.4-8.7 21s2.9 15.2 8.7 21c5.8 5.8 13.4 8.7 21 8.7h148.6c32.8 0 59.4-26.6 59.4-59.4V720c0-7.6-2.9-15.2-8.7-21-5.8-5.8-13.4-8.7-21-8.7zM401.8 580.1L155.4 826.5V720c0-7.6-2.9-15.2-8.7-21-5.8-5.8-13.4-8.7-21-8.7s-15.2 2.9-21 8.7c-5.8 5.8-8.7 13.4-8.7 21v148.6c0 32.8 26.6 59.4 59.4 59.4H304c7.6 0 15.2-2.9 21-8.7 5.8-5.8 8.7-13.4 8.7-21s-2.9-15.2-8.7-21c-5.8-5.8-13.4-8.7-21-8.7H197.5l246.4-246.4c11.6-11.6 11.6-30.4 0-42-11.6-11.7-30.5-11.7-42.1-0.1zM197.5 155.4H304c7.6 0 15.2-2.9 21-8.7 5.8-5.8 8.7-13.4 8.7-21s-2.9-15.2-8.7-21c-5.8-5.8-13.4-8.7-21-8.7H155.4C122.6 96 96 122.6 96 155.4V304c0 7.6 2.9 15.2 8.7 21 5.8 5.8 13.4 8.7 21 8.7s15.2-2.9 21-8.7c5.8-5.8 8.7-13.4 8.7-21V197.5l246.4 246.4c11.6 11.6 30.4 11.6 42 0 11.6-11.6 11.6-30.4 0-42L197.5 155.4z" stroke="transparent"></path>
          </svg>
        </div>
        

        <el-dropdown trigger="click" @command="dropdownComm">
          <!-- <a>{{ id }}</a> -->
          <span class="el-dropdown-link">
            
            <img src="../assets/images/genshin.png" alt="adminLogo">
            <!-- <i class="el-icon-arrow-down el-icon--right"></i> -->
          </span>
          <el-dropdown-menu slot="dropdown">
            <!-- <el-dropdown-item>个人中心</el-dropdown-item> -->
            <el-dropdown-item command="lock">锁定系统</el-dropdown-item>
            <el-dropdown-item command="logout">退出系统</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <!-- <div class="admin-logo">
          <img src="../assets/images/genshin.png" alt="adminLogo">
        </div> -->
      </div>
    </div>
  </el-header>
</template>

<script>
import bus from '../assets/bus.js';
export default {
  data(){
    // id = "用户名";
    return {
      bFullScreen:false,
      asideStatus:false,
    }
  },
  mounted(){
    this.asideStatus=window.localStorage.getItem('asideStatus');
    if(this.asideStatus===null){
      this.asideStatus=false;
    }else{
      this.asideStatus=this.asideStatus==='false'?false:true;
    }
    // console.log(this.asideStatus)
  },
  methods:{
    fFullScreen(){    //全屏事件
      let element = document.documentElement;
      if(this.bFullScreen){
        if (document.exitFullscreen) {
          document.exitFullscreen();
        } else if (document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen();
        } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
        } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
        }
      }else{
        if (element.requestFullscreen) {
          element.requestFullscreen();
        } else if (element.webkitRequestFullScreen) {
            element.webkitRequestFullScreen();
        } else if (element.mozRequestFullScreen) {
            element.mozRequestFullScreen();
        } else if (element.msRequestFullscreen) {
            // IE11
            element.msRequestFullscreen();
        }
      }
      this.bFullScreen=!this.bFullScreen;
    },
    dropdownComm(command){
      switch(command){
        case 'lock':   //锁定系统
          window.localStorage.setItem('lock',true);
          this.$message({
            showClose:true,
            message: '系统已被锁定',
            type: 'success'
          })
          this.$router.push({path:'/lock'})
        break;
        case 'logout':   //退出系统
          this.$http.post('http://10.21.40.155:3000/logout').then(res=>{
            if(res.data.code===200){
              this.$message({
                showClose:true,
                message:res.data.msg,
                type:'success'
              })
              window.localStorage.removeItem('loginToken');
              this.$router.push({path:'/login'})
            }else{
              this.$message({
                showClose:true,
                message:res.data.msg,
                type:'error'
              })
            }
          })
        break;
      }
    },
    changeAside(){
      this.asideStatus=!this.asideStatus;
      if(this.asideStatus){
        // console.log(1)
        bus.$emit('changeStatus',true)
        window.localStorage.setItem('asideStatus',true)
      }else{
        console.log(2)
        bus.$emit('changeStatus',false)
        window.localStorage.setItem('asideStatus',false)
      }
    },
    goHome(){
      this.$router.push({path:'/'})
    }
  }
}
</script>


<style lang="less" scoped>

  .display(){
    display: flex;
    align-items: center;
    justify-content: space-between;
    

  }
  .header {
    
    .display()
    
    
    // background: pink;
  }
  .header-left {
    
    max-width: 280px;
    .display();
    
    margin-top: 2px;

    img {
      width: 60px;
      height: 60px;
      cursor: pointer;
      transition: all 0.3s ease;

      &:hover {
        text-decoration: none; /* 移除下划线 */
        transform: translateY(-2px); /* 轻微的向上移动效果 */
        }
    }
  }
  .header-right {
    .display();
    margin-top: 7px;
    img {
      width: 40px;
      height: 40px;
      margin-top: 3px;
      margin-left: 10px;
      border-radius: 50%;
    }

  
    .svg-icon-div {
      height: 100%;
      padding: 0 22px;
    }
  }

  .header-middle {

      .display();
    img {
      position: absolute;
      top: 7px; /* 距离顶部的距离，可以根据需要调整 */
      left: 50%; /* 水平居中定位 */
      transform: translateX(-50%); /* 用于确保图片准确居中 */
      width: 180px; /* 图片宽度，根据需要调整 */
      height: auto; /* 高度自动，保持图片宽高比 */
      transition: .3s;
      cursor: pointer;
      &:hover {
        transform: translateX(-50%) translateY(-2px);

      }
    }

  }

  .svg-icon {
    vertical-align: middle;
    color: #fff !important;
    fill: #515151;
    cursor: pointer;
    transition: .3s;
    
    &:hover {
      fill: #58A6F3;
      transform: translateY(-2px);

    }
  }

  .header-homepage {
  font-family: 'Segoe UI', Tahoma,  Verdana, sans-serif; /* 优雅的字体族 */
  font-size: 18px; /* 适中的字体大小 */
  color: #000000; /* 舒适的蓝色调 */
  font-weight: 600; /* 字体加粗 */
  padding: 10px 20px; /* 文本周围的空间 */
  display: flex; /* 使用flexbox进行布局 */
  align-items: center; /* 垂直居中 */
  justify-content: center; /* 水平居中 */
  // background: #FFFFFF; /* 背景颜色 */
  border-radius: 5px; /* 轻微的圆角边框 */
  // box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1); /* 细微的阴影效果 */
  transition: all 0.3s ease; /* 鼠标悬浮过渡效果 */
}

.header-homepage:hover {
  color: #437ff6; /* 鼠标悬浮时的字体颜色 */
  // background: #4A90E2; /* 鼠标悬浮时的背景颜色 */
  text-decoration: none; /* 移除下划线 */
  // transform: translateY(-2px); /* 轻微的向上移动效果 */
}
  .svg-icon2{
    vertical-align: middle;
    color: #000000 !important;
    fill: #515151;
    cursor: pointer;
    transition: .3s;
    &:hover {
      fill: #58A6F3;
    }
  }

  .icon-menu {
    display: block;
    width: 25px;
    height: 25px;
    margin-top : 2px;
    margin-left: 3px;
    margin-right:5px;
    background: url("../assets/images/menu.png") no-repeat;
    background-size: 100% 100%;
    cursor: pointer;
    transition: .3s;
    &:hover {
      background: url("../assets/images/menu_hover.png") no-repeat ;
      background-size: 100% 100%;
      transform: translateY(-1px);
    }
  }


  .menu_click{
    display: flex;

    align-items: center; /* 垂直居中对齐子元素 */
    
    // width: 25px;
    // height: 25px;
    // margin: 0px 15px;
    margin-top : 2px;

    background-size: 100% 100%;
    cursor: pointer;
    transition: .3s;
    &:hover {

      background-size: 100% 100%;
      transform: translateY(-1px);
    }
  }
  .el-icon-s-fold{
    display: block;
    width: 60px;
    height: 60px;
    cursor: pointer;
  }
</style>

