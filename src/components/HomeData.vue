<template>
  <div class="home-data el-row">
    <el-row class="data-items" :gutter="20">
      <el-col class="item" :xs="12" :sm="12" :lg="6">
        <div class="small-item small-item1 el-col el-col-24 el-col-xs-12 el-col-sm-12 el-col-lg-6">
          <div class="small-item-left small-item1-left">
            <icon class="icon" name="user" :scale="5"></icon>
          </div>
          <div class="small-item-right small-item1-right">
            <div class="number">{{userNumber}}</div>
            <div class="item-des">今日入住</div>
          </div>
        </div>
      </el-col>
      </el-row>
      <el-row class="data-items" :gutter="20">
      <el-col class="item" :xs="12" :sm="12" :lg="6">
        <div class="small-item small-item1 el-col el-col-24 el-col-xs-12 el-col-sm-12 el-col-lg-6">
          <div class="small-item-left small-item2-left">
            <icon class="icon" name="room" :scale="5"></icon>
          </div>
          <div class="small-item-right small-item2-right">
            <div class="number">{{roomNumber}}</div>
            <div class="item-des">房间已开</div>
          </div>
        </div>
      </el-col>
      </el-row>
      <el-row class="data-items" :gutter="20">
      <el-col class="item" :xs="12" :sm="12" :lg="6">
        <div class="small-item small-item1 el-col el-col-24 el-col-xs-12 el-col-sm-12 el-col-lg-6">
          <div class="small-item-left small-item3-left">
            <icon class="icon" name="money" :scale="5"></icon>
          </div>
          <div class="small-item-right small-item3-right">
            <div class="number">{{money}}</div>
            <div class="item-des">收入盈利</div>
          </div>
        </div>
      </el-col>
      </el-row>
      <el-row class="data-items" :gutter="20">
      <el-col class="item" :xs="12" :sm="12" :lg="6">
        <div class="small-item small-item1 el-col el-col-24 el-col-xs-12 el-col-sm-12 el-col-lg-6">
          <div class="small-item-left small-item4-left">
            <icon class="icon" name="left" :scale="5"></icon>
          </div>
          <div class="small-item-right small-item4-right">
            <div class="number">{{surplusRooms}}</div>
            <div class="item-des">剩余房间</div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userNumber:19,   //今日入住
      roomNumber:19,   //房间已住
      money:114514,    //收入盈利
      surplusRooms:810,    //剩余房间数
    }
  },
  mounted() {
    this.getData();
  },
  
  methods:{
    getData() {
      this.$http.get('http://10.21.40.155:3000/getHomeData').then(res=>{
        if(res.data.code===200){
          this.userNumber=res.data.users;
          this.roomNumber=res.data.rooms;
          this.surplusRooms=res.data.surplusRooms;
          res.data.money.map((item)=>{
            this.money+=Number(item.income)
          })
        }
      })
    },
  }
}
</script>


<style lang="less" scoped>
  .disflex() {
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }
  .column() {
    .disflex();
    flex-direction: column;
  }
  .data-items {
    display: flex;
    // flex-wrap: wrap;
    overflow: hidden;
  }
  .item {
    width: 25%;
    height: 130px;
    padding: 0 10px;
    box-sizing: border-box;
  }
  .itemStyle(@bg) {
    width: 100%;
    height: 100%;
    display: flex;
    &-left {
      width: 130px;
      height: 100%;
      background: @bg;
      border-top-left-radius: 5px;
      border-bottom-left-radius: 5px;
      .disflex();
    }
    &-right {
      flex: 1;
      height: 100%;
      background: #fff;
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px;
      color: #99a9c0;
      .column();
    }
    &:hover {
      box-shadow: 0 1px 6px rgba(0,0,0,.2);
    }
  }
  .small-item1 {
    .itemStyle(#41B883);
  }
  .small-item2 {
    .itemStyle(#9AB7E0);
  }
  .small-item3 {
    .itemStyle(#F7C94D);
  }
  .small-item4 {
    .itemStyle(#E45F5F);
  }
  .number {
    font-size: 30px;
    padding-bottom: 6px;
  }
  @media only screen and(min-width: 768px){
    .item {
      width: 50%;
    }
    .item:nth-child(3),.item:nth-child(4) {
      margin-top: 15px;
    }
  }
  @media only screen and(min-width: 1200px){
    .item {
      width: 38%;
      justify-content: left;
      
    }
    .item:nth-child(3),.item:nth-child(4) {
      margin-top: 50px;
    }
  }



  /* 使用新的媒体查询 */
// @media only screen and (max-width: 767px) {
//   .item {
//     width: 100%; /* 在较小屏幕上每行一个 */
//   }
//   .item:nth-child(n+2) {
//     margin-top: 20px; /* 从第二个项目开始添加上边距 */
//   }
// }

// @media only screen and (min-width: 768px) {
//   .item {
//     width: 40%; /* 在较大屏幕上每行两个 */
//   }
//   .item:nth-child(odd) {
//     margin-right: 20px; /* 为左侧的项添加右边距 */
//   }
//   .item:nth-child(even) {
//     margin-left: 20px; /* 为右侧的项添加左边距 */
//   }
//   .item:nth-child(n+3) {
//     margin-top: 120px; /* 从第三个项目开始添加上边距 */
//   }
// }

  .el-row {
    margin: 25px 0;
    margin-top: 20px;
    margin-bottom: 40px;
    justify-content: left;
  }

  .el-col{
    margin-left:20px;
    margin-right: 50px;
  }

  

</style>
