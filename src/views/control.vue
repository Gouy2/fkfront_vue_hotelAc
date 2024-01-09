<template>
  <div> 
    <Header></Header>

    <div class="aircon">
      <button class="button_id">
        <span class = "aircon-id">ID : {{ airConditionerId }}</span>
      </button>
      
    </div>

  <div class="box1" @click="handleClick">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
       
          <span>用户控制面板</span>       

          <div class = "switch">
          <!-- <svg t="1700661854833" viewBox="0 0 1024 1024" :class="{'is-on': isOn}" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6332" xmlns:xlink="http://www.w3.org/1999/xlink" width="30" height="30">
            <path d="M512 576c-25.6 0-42.666667-17.066667-42.666667-42.666667V85.333333c0-25.6 17.066667-42.666667 42.666667-42.666666s42.666667 17.066667 42.666667 42.666666v448c0 25.6-17.066667 42.666667-42.666667 42.666667z" :fill="isOn ? '#970212' : '#000000'"  p-id="6333" ></path>
            <path d="M512 981.333333C264.533333 981.333333 64 780.8 64 533.333333c0-140.8 64-268.8 174.933333-354.133333 17.066667-12.8 42.666667-8.533333 59.733334 8.533333 12.8 17.066667 12.8 46.933333-8.533334 59.733334a359.68 359.68 0 0 0-140.8 285.866666c0 200.533333 162.133333 362.666667 362.666667 362.666667s362.666667-162.133333 362.666667-362.666667c0-110.933333-51.2-217.6-140.8-285.866666-17.066667-12.8-21.333333-42.666667-8.533334-59.733334 12.8-17.066667 42.666667-21.333333 59.733334-8.533333 110.933333 85.333333 174.933333 213.333333 174.933333 354.133333C960 780.8 759.466667 981.333333 512 981.333333z" :fill="isOn ? '#970212' : '#000000'"  p-id="6334">

          </path></svg> -->
            <!-- <img src="../assets/images/switch.svg" @click="togglePower" /> -->
            <el-switch
            v-model="isOn"
            @click="togglePower" >
            </el-switch>

        </div>
        <!-- <div>
          <h3>空调id</h3>
          <el-tag type="info">{{ airConditionerId }}</el-tag>
        </div> -->
      </div>

      <!-- 第一行 -->
      <el-row>
        
        
        <el-col :span="12" >
            <h3>当前温度</h3>
            <el-tag>{{ Math.round(now_temperature) }}°C</el-tag>
        </el-col>
        
          
      <!-- 温度控制 -->
      <el-col :span="12" >
        <h3>目标温度</h3>
        <el-button-group>
            <el-button icon="el-icon-minus" @click="changeTemperature(-1)"></el-button>
            <el-button>{{ tar_temperature }}°C</el-button>
            <el-button icon="el-icon-plus" @click="changeTemperature(1)"></el-button>
        </el-button-group>
      </el-col>

     
      
      </el-row>

      <el-row>       
          

        <!-- 风速控制 -->
        <el-col :span="12"  >
          <h3>风速控制</h3>
          <el-select v-model="fanSpeed" placeholder="请选择" @change="handleClick" style="font-size: 28px;">
              <el-option label="低速" value= '低速' ></el-option>
              <el-option label="中速" value= '中速'></el-option>
              <el-option label="高速" value= '高速' ></el-option>
          </el-select>         
        </el-col>

        <el-col :span="12" >
          <h3>空调模式</h3>
          <el-select v-model="acmodel" placeholder="请选择"  @change="handleClick">
              <el-option label="制冷" value="cold"></el-option>
              <el-option label="制热" value="hot"></el-option>
          </el-select>         
        </el-col>
      </el-row>

      <!-- 第二行 -->
      <el-row >
        <!-- 当前费用 -->
        <el-col :span="12">
          <h3>当前费率</h3>
          <el-tag type="warning">{{ currentCost }}元/1°C</el-tag>
        </el-col>

        <!-- 累计费用 -->
        <el-col :span="12">
          <h3>累计费用</h3>
          <el-tag type="success">{{ totalCost }}元</el-tag>
        </el-col>
        
      </el-row>
      
    </el-card>
  </div>

</div>
</template>


<script>

import Header from '@/components/Header.vue'; 



export default {

  components: {
    Header
  },
  data() {
    return {
      isOn: false,
      tar_temperature: 22,
      fanSpeed: '低速',
      acmodel: 'cold',
      currentCost: 1,
      totalCost: 0,
      airConditionerId:1,
      now_temperature: 15,
      tempLow: 18,
      tempUp: 28,
    };
  },

  created() {
    this.airConditionerId = this.$route.params.id; 
    
    
    let storedSettings = localStorage.getItem('temperatureSettings');
    if (storedSettings) {
      let temperatureSettings = JSON.parse(storedSettings);
      this.tempLow = temperatureSettings.tempLow;
      this.tempUp = temperatureSettings.tempUp;
    } else {
      localStorage.setItem('temperatureSettings', JSON.stringify({ tempLow: 18, tempUp: 28 }));
    }

    this.interval = setInterval(this.fetchAirConditionerData, 5000); // 每秒更新一次数据

  },

  mounted() {
    window.addEventListener('storage', this.handleStorageChange); // 监听 localStorage 变化
  },
    
  methods: {

  handleClick() {
      this.updateAirConditionerSettings();
      
      console.log('温度下限：',this.tempLow,"温度上限：",this.tempUp);


  },

  handleStorageChange(event) {
    if (event.key === 'temperatureSettings') {
      let temperatureSettings = JSON.parse(event.newValue);
      this.tempLow = temperatureSettings.tempLow;
      this.tempUp = temperatureSettings.tempUp;
    }
  },

  togglePower() {
      this.isOn = !this.isOn;
      // 添加逻辑来处理空调的开关
  },

  
    changeTemperature(value) {

      const newTemperature = this.tar_temperature + value; 
      if (newTemperature >= this.tempLow && newTemperature <= this.tempUp) {  // 判断温度是否在范围内
      this.tar_temperature = newTemperature;
      } else {

        this.$message({ // 提示温度超出范围
          message: '温度超出设定范围',
          type: 'warning'
        });
      }
    },

    fetchAirConditionerData() { // 获取空调数据

      this.$axios.post('/update_status', { room_id: this.airConditionerId.toString() })
        .then(response => {
          const data = response.data;
          this.isOn = data.is_on;
          this.tar_temperature = data.tar_temp;
          this.fanSpeed = this.mapFanSpeed(data.wind); // 将接口返回的风速转换为 'low', 'medium', 'high'
          this.acmodel = data.mode, 
          this.totalCost = data.now_cost;
          this.now_temperature = data.cur_temp;
          // 费率 需要从接口获取或者计算

          console.log('</update_status> successfully!');

          console.log(response.data);
        })
        .catch(error => {
          console.error('Error fetching air conditioner data:', error);
        });

      // console.log('获取空调数据');
    },

    updateAirConditionerSettings() { // 更新空调设置 click
      const updata = {
        room_id: this.airConditionerId,
        is_on: this.isOn,
        mode: this.acmodel,  // 确保这里的值是 'hot' 或 'cold'
        tar_temp: this.tar_temperature,
        wind: this.mapSpeedToNumber(this.fanSpeed), 


      };
      
      console.log("空调状态：",updata);

      this.$axios.post('/control_ac', updata)
        .then(response => {
          console.log('</control_ac> response successfully');
          console.log('更新空调数据');

        })
        .catch(error => {
          console.error('Error updating settings: ', error);
        });
      // console.log('更新空调数据');
    },

    mapFanSpeed(windValue) {
      // 将数字风速映射为字符串
      const speedMap = { 1: '低速', 2: '中速', 3: '高速' };
      return speedMap[windValue] || '中速'; // 默认为 'medium'
    },

    mapSpeedToNumber(speedString) {
    // 将字符串风速映射回数字
    const numberMap = { '低速': 1, '中速': 2, '高速': 3 };
    return numberMap[speedString] || 2; // 默认为 2
    },

    mapAcModel(mode) {
    const modeMap = {
      '制热': 'hot',
      '制冷': 'cold'
    };
    return modeMap[mode] || 'cold'; // 默认为 'cold'
    },
    
    mapModeToNumber(mode) {
    // 将文字模式映射为数字
    const numberMap = { 'hot': '制热', 'cold': '制冷' };
    return numberMap[mode] || '制冷'; // （制冷）
    },
    
  
  },

  
  beforeDestroy() { // 组件销毁时清除定时器
    clearInterval(this.interval);
    window.removeEventListener('storage', this.handleStorageChange);
  },


  
  watch: {
    // 监控空调使用情况来计算费用
    isOn(newVal) { 
      if (newVal) {
        // 开始计算费用
      } else {
        // 停止计算并累加到总费用
      }
    }
  }
};
</script>


<style  lang = "less" scoped>

  .disflex() {
    display: flex;
    justify-content: center;
    align-items: center;
  }

.aircon {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}

.aircon-id {
  font-size: 20px;
  transition: all 0.3s ease;
  &:hover {
        // 
        font-size: 18px;
        text-decoration: none; /* 移除下划线 */
        // transform: translateY(-3px); /* 轻微的向上移动效果 */
        }
}

.clearfix {
  display: flex ; 
  align-items: center; /* 垂直居中对齐子元素 */
  justify-content: center;
  // justify-content: start; /* 子元素向行的起始位置对齐 */
  // text-align: center;
  // width: 100%;
  font-size: 24px; 
  font-weight: bold;
  padding: 20px 0; 
  background-color: #f6f8fa; /* 背景颜色 */
  border-bottom: 1px solid #eaeaea; /* 分隔线条 */
}
.clearfix span, .clearfix .switch {
  margin-right: 10px; /* 在元素之间添加间隔 */
}

.switch{
      width: 30px;  
      height: auto;
      margin-top: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
      &:hover {
        text-decoration: none; /* 移除下划线 */
        // transform: translateY(-2px); /* 轻微的向上移动效果 */
        }
}

.box-card {
width: 100%;
height: 80vh;
max-width: 800px; /* 或根据你的需求调整最大宽度 */
margin: 1vh auto;
box-shadow: 0 2px 12px 0 rgba(0,0,0,0.1);
border-radius: 20px;
padding: 20px;
background-color: #ffffff;
box-sizing: border-box; /* 确保内边距不影响宽度 */
background-size: cover; /* 背景图片覆盖整个元素 */
background-position: center; /* 背景图片居中显示 */

box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* 添加微妙的阴影 */
  transition: box-shadow 0.3s ease-in-out;
  &:hover {
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.35); /* 鼠标悬停时阴影加深 */
  }
}

/* 调整栅格布局的样式 */
.el-row {
  margin-bottom: 30px;
  // margin-top: 20px;
  text-align: center; /* 居中对齐子元素 */
  border-bottom: 1px solid #eaeaea; /* 给每个列表项底部添加边框 */
  padding: 15px 0; /* 在列表项上下添加间距 */

}

.el-col {
  display: flex;
  flex-direction: column;
  align-items: center; /* 垂直居中 */
  justify-content: center; /* 水平居中 */

  border-right: 1px solid #eaeaea;
  &:last-child {
    border-right: none; /* 移除最后一列的边框 */
  }
}

/* 图片样式 */
img {
  max-width: 100%; /* 确保图片不超过容器宽度 */
  height: auto; /* 自动调整高度 */
  cursor: pointer; /* 将鼠标指针变为手形 */
  margin-bottom: 10px;
}

/* 控制元素样式 */
h3 {

  font-size: 18px; /* 适合的字体大小 */
  color: #333;
  margin-bottom: 15px; /* 增加底部间距 */
  text-transform: uppercase; /* 文本大写 */
  letter-spacing: 1px; /* 字母间距 */
}

.el-button {
  margin-top: 1px; /* 为按钮添加上间距 */
  
  background-color: #f6f8fa; /* 设置为与 el-tag 相同的背景色 */
  color: #333; /* 设置为与 el-tag 相同的文字颜色 */
  // border: none; /* 移除边框 */
  // box-shadow: none; /* 移除阴影 */
  border-radius: 4px; /* 设置为与 el-tag 相同的圆角大小 */
  padding: 5px 10px; /* 根据需要调整内边距 */
  height: 37px; /* 如果需要，调整高度 */
  line-height: normal;  /* 根据需要调整行高 */
  font-size: 16px; /* 根据需要调整字体大小 */
  cursor: pointer; /* 设置鼠标样式 */
}



/* 标签样式 */
.el-tag {
  margin: 5px 0;
  font-size: 16px; /* 适合的字体大小 */
  color: #333;
  margin-bottom: 15px; /* 增加底部间距 */
  text-transform: uppercase; /* 文本大写 */
  // letter-spacing: 1px; /* 字母间距 */
}

.el-select{
  margin-bottom: 30px;
  width: 100px;
  text-align: center;
  color: #606266;
}

.el-select .el-input__inner {
  text-align: center; /* 使文本居中 */
  font-size: 20px;
}



.el-select-dropdown .el-select-dropdown__item {
  text-align: center; /* 下拉选项文本居中 */
}

.el-col img {
width: 100%; /* 使图片宽度响应式 */
max-width: 80px; /* 设置一个最大宽度 */
height: auto; /* 保持图片的宽高比 */
}

.button_id {
 position: relative;
 height: 50px;
 padding: 0 30px;
 border: 2px solid #000;
 background: #f1f2f7;
 user-select: none;
 white-space: nowrap;
 transition: all .05s linear;
 font-family: inherit;
}

.button_id:before, button:after {
 content: "";
 position: absolute;
 background: #f1f2f7;
 transition: all .2s linear;
}

.button_id:before {
 width: calc(100% + 6px);
 height: calc(100% - 16px);
 top: 8px;
 left: -3px;
}

.button_id:after {
 width: calc(100% - 16px);
 height: calc(100% + 6px);
 top: -3px;
 left: 8px;
}

.button_id:hover {
 cursor: pointer;
}

.button_id:active {
 transform: scale(0.95);
}

// .button_id:hover:before {
//  height: calc(100% - 32px);
//  top: 16px;
// }

// .button_id:hover:after {
//  width: calc(100% - 32px);
//  left: 16px;
// }

.button_id span {
 font-size: 15px;
 z-index: 3;
 position: relative;
 font-weight: 600;
}

.el-switch{
  margin-bottom: 10px;
}

</style>

