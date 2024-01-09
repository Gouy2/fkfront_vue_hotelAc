<template>
  <div class="main-content">
    <topData />
    

    <div class="log-card-container">
    <el-card class="log-card" shadow="always">
      <h2>酒店日志</h2>
      <div v-for="log in logs" :key="log.id" class="log-entry">
        <h3>{{ log.title }}</h3>
        <p>{{ log.date }} - {{ log.description }}</p>
      </div>
    </el-card>
  </div>

    

  </div>
</template>

<script>

import topData from './HomeData'

export default {
  data(){
    return {
      newOrder:[],
      logs: [
      { 
          id: 1, 
          title: '前日入住', 
          date: '2023-12-06', 
          description: '前日入住客人数：38人。' 
        },
        { 
          id: 2, 
          title: '前日收入', 
          date: '2023-12-06', 
          description: '前日总收入：￥8,600。' 
        },
        { 
          id: 3, 
          title: '空调调用', 
          date: '2023-12-06', 
          description: '空调调用次数：75次，平均温度设置：24°C。' 
        },
        { 
          id: 4, 
          title: '维修情况', 
          date: '2023-12-06', 
          description: '维修项目：客房401空调，完成情况：已修复。' 
        },
        { 
          id: 5, 
          title: '客户反馈', 
          date: '2023-12-06', 
          description: '接收到3条客户反馈，均提及提高客房清洁度。' 
        },
        { 
          id: 6, 
          title: '昨日入住', 
          date: '2023-12-07', 
          description: '昨日入住客人数：42人。' 
        },
        { 
          id: 7, 
          title: '昨日收入', 
          date: '2023-12-07', 
          description: '昨日总收入：￥9,800。' 
        },
        { 
          id: 8, 
          title: '空调调用', 
          date: '2023-12-07', 
          description: '空调调用次数：88次，平均温度设置：22°C。' 
        },
        
      ]
    }
  },
  components:{
    topData,

  },
  
}
</script>


<style lang="less" scoped>
  .main-content {
    width: 100%;
    height: 100%;
    // background: #fff;
    //居中
    overflow: hidden;
    
  }
  .center {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .el-row {
    margin-bottom: 15px;
  }

  .log-card-container {
  position: fixed;
  top: 105px;
  right: 120px;
  width: 600px;
}

.log-card {
  height: auto;
  overflow: hidden;
  border: 1px solid #ebeef5;
  border-radius: 4px;
}

.log-entry {
  margin-bottom: 10px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

h2 {
  text-align: center;
  color: #303133;
  margin-bottom: 20px;
}

h3 {
  margin-top: 0;
  color: #303133;
}
</style>
    

