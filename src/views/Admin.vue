<template>
<div class = "headerTest">
  <div>

    <div class="setting1">
      <el-button type="primary" @click="dialogFormVisible = true" class="testbutton1">空调模式设置</el-button>
    </div>
    <el-dialog :title="formTitle" :visible.sync="dialogFormVisible">
      <el-form :model="form" :rules="rules" ref="form">
        
        <el-form-item label="设置空调模式" :label-width="formLabelWidth" prop="airconType">
          <el-select v-model="form.airconType" placeholder="请设置空调工作模式">
            <el-option label="制冷" value="制冷"></el-option>
            <el-option label="制热" value="制热"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="温控下限" :label-width="formLabelWidth" prop="tempLow">
          <el-input v-model="form.tempLow" class="moneyinput" autocomplete="off">
            <template slot="append">°C</template>
          </el-input>
        </el-form-item>
        <el-form-item label="温控上限" :label-width="formLabelWidth" prop="tempUp">
          <el-input v-model="form.tempUp" class="moneyinput"  autocomplete="off">
            <template slot="append">°C</template>
          </el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="saveChanges">确 定</el-button>
      </div>
    </el-dialog>

    <div class="setting2">
      <el-button type="primary" @click="dialogFormVisible2 = true" class="testbutton2">空调费率设置</el-button>
    </div>
    <el-dialog :title="formTitle2" :visible.sync="dialogFormVisible2">
      <el-form :model="form2" :rules="rules2" ref="form">
        
        <el-form-item label="低速风费率" :label-width="formLabelWidth" prop="speed1">
          <el-input v-model="form2.speed1"  class="moneyinput" autocomplete="off">
            <template slot="append">元/1°C</template>
          </el-input>
        </el-form-item>
        <el-form-item label="中速风费率" :label-width="formLabelWidth" prop="speed2">
          <el-input v-model="form2.speed2" class="moneyinput" autocomplete="off">
            <template slot="append">元/1°C</template>
          </el-input>
        </el-form-item>
        <el-form-item label="高速风费率" :label-width="formLabelWidth" prop="speed3">
          <el-input v-model="form2.speed3" class="moneyinput" autocomplete="off">
            <template slot="append">元/1°C</template>
          </el-input>
        </el-form-item>
        <el-form-item label="计费费率" :label-width="formLabelWidth" prop="cost_rate">
          <el-input v-model="form2.cost_rate" class="moneyinput" autocomplete="off">
            <template slot="append">元/1°C</template>
          </el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible2 = false">取 消</el-button>
        <el-button type="primary" @click="sendRate">确 定</el-button>
      </div>
    </el-dialog>

    <!-- <el-select v-model="fanSpeed" placeholder="设置空调计费费率">
              <el-option label="低速" value="low"></el-option>
              <el-option label="中速" value="medium"></el-option>
              <el-option label="高速" value="high"></el-option>
    </el-select> -->
    <div class="switch" @click="startRun">
      <el-button>启动</el-button>
    </div>

  </div>
 <div>

 </div>
  <div class = "container">
    
    <div class = "content">
      <el-row class="info-cards" >
      <el-col :span="4.8" v-for="(item, index) in pagedAirConditioners" :key="index" >
        <el-card >
          <i class="el-icon-setting"  @click="ToControl(item.id)"></i>
          
          <div class="status-tag">
            <el-tag :type="item.isOn ? 'success' : 'info'">{{ item.isOn ? '开机' : '关机' }}</el-tag>
          </div>
          <h4>房间ID: {{ item.id }}</h4>
          <h4>是否入住: {{ item.is_ava? '否' : '是' }}</h4>
          <h4>工作模式: {{ item.isOn ? item.mode : '— —' }}</h4>
          <h4>当前室温: {{ item.isOn ? Math.round(item.currentTemp) + '°C' : '— —' }}</h4>
          <h4>目标温度: {{ item.isOn ? item.targetTemp + '°C' : '— —' }}</h4>
          <h4>当前风速: {{ item.isOn ? item.fanSpeed : '— —' }}</h4>
          <!-- <h4>当前费率: {{ item.isOn ? item.currentCost + '元/1°C' : '— —' }}</h4> -->
          <h4>累积费用: {{ item.isOn ? item.totalCost + '元' : '— —' }}</h4>
        </el-card>
      </el-col>
    </el-row>

    <div class="pagination">
      <el-pagination
      @current-change="handlePageChange"
      :current-page="currentPage"
      :page-size="10" 
      layout="prev, pager, next"
      :total="airConditioners.length">
    </el-pagination>
    </div> 
    
    </div> 
    
  </div>
</div>
</template>


<script>

export default {



  data() {
    return {
      currentPage: 1,
      pageSize: 10, // 每页显示10条记录
      
      airConditioners: [],
      updateInterval: null,

      dialogFormVisible: false,
      dialogFormVisible2: false,
      formTitle: '空调模式设置',
      formTitle2: '空调费率设置',
      formLabelWidth: '120px',
      form: {
        airconType: '',
        tempLow: '',
        tempUp: ''
      },
      rules: {
        airconType: [
          { required: true, message: '请选择空调工作模式', trigger: 'blur' },
        ],
        tempLow: [
          { required: true, message: '请输入温控下限', trigger: 'blur' },
        ],
        tempUp: [
          { required: true, message: '请输入温控上限', trigger: 'blur' },
        ]
      },
      form2: {
        speed1: '',
        speed2: '',
        speed3: '',
        cost_rate: ''
      },
      rules2: {
        speed1: [
          { required: true, message: '请输入低速风费率', trigger: 'blur' },
        ],
        speed2: [
          { required: true, message: '请输入中速风费率', trigger: 'blur' },
        ],
        speed3: [
          { required: true, message: '请输入高速风费率', trigger: 'blur' },
        ],
        cost_rate: [
          { required: true, message: '请输入计费费率', trigger: 'blur' },
        ]
      },

    };
  },

  watch: {
  'form.airconType'(newValue) {
    if (newValue === '制冷') {
      this.form.tempLow = 16;
      this.form.tempUp = 24;
      }

    else if (newValue === '制热') {
      this.form.tempLow = 18; 
      this.form.tempUp = 25; 
      }
    }
  },

  computed: {
    pagedAirConditioners() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = this.currentPage * this.pageSize;
      return this.airConditioners.slice(start, end);
    },
  },

  // mounted() {
  //   this.airConditioners = this.initializeData(); // 初始化数据
  // },

  created(){
    this.airConditioners = this.initializeData(); // 初始化数据
    this.updateInterval = setInterval(this.updateAirConditioners, 5000); // 每秒更新一次空调数据
  },

  beforeDestroy() {
    clearInterval(this.updateInterval); // 销毁组件前清除定时器
  },

  methods: {

    handlePageChange(newPage) {
      this.currentPage = newPage;
    },

    testClick(){
      console.log("T T")
    },

    ToControl(AirId){
      console.log("test")
      this.$router.push({ name: 'Control', params: { id: AirId }});
    },

    saveChanges() {

    // 保存设置到本地存储
    localStorage.setItem('temperatureSettings', JSON.stringify({
      tempLow: this.form.tempLow,
      tempUp: this.form.tempUp
    }));

    this.$refs.form.validate((valid) => {
      if (valid) {
        // 表单验证通过
        const { airconType, tempLow, tempUp } = this.form;

        this.saveForm({ airconType, tempLow, tempUp });
        this.dialogFormVisible = false; // 关闭对话框
        console.log('form验证成功！');
      } else {
        // 表单验证失败
        console.log('表单验证失败！');
      }
    });



    },
    saveForm(formData) {
      //令所有空调的工作模式为formData.airconType

      this.airConditioners.forEach(ac => {  // 遍历所有空调
        ac.mode = formData.airconType;
      });
      
      console.log('保存的数据', formData);
    },

    initializeData() {
      // 生成40个空调数据
      const airConditioners = [];
      for (let i = 1; i <= 40; i++) {
        airConditioners.push({
          id: i,
          isOn: false,
          is_ava: true,
          // mode: ['制冷', '制热',][Math.floor(Math.random() * 2)],
          mode: '制热',
          // currentTemp: Math.floor(Math.random() * 10) + 16,
          currentTemp: 15,
          // targetTemp: Math.floor(Math.random() * 10) + 20,
          targetTemp: 22,
          // fanSpeed: ['低速', '中速', '高速'][Math.floor(Math.random() * 3)],
          fanSpeed: '中速',
          currentCost: 1 ,
          totalCost: 0,
        });
      }
      return airConditioners;
    },

    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    },
    

     updateAirConditioners() {
      // console.log('轮询更新信息success');
      this.airConditioners.forEach(ac => {  // 遍历id<5的空调
        if ( ac.id < 6) { 
          console.log("更新信息的空调id:",ac.id);
          this.updateAirConditionerInfo(ac.id); 
          
          // await this.sleep(2000); // 等待2000毫秒（2秒）

        }
      });
    },

    updateAirConditionerInfo(id) {
      this.$axios.post('/update_status', {  room_id: id.toString() })
        .then(response => {
          console.log('success', response.data);

          const index = this.airConditioners.findIndex(ac => ac.id === id); // 找到该空调在数组中的索引
          if (index !== -1) { 
            
            // this.$set(this.airConditioners[index], 'isOn', data.is_on);
            // this.$set(this.airConditioners[index], 'mode', this.mapAcModel(data.mode));
            // this.$set(this.airConditioners[index], 'targetTemp', data.tar_temp);
            // this.$set(this.airConditioners[index], 'currentTemp', data.cur_temp);
            // this.$set(this.airConditioners[index], 'fanSpeed', this.mapFanSpeed(data.wind));
            // this.$set(this.airConditioners[index], 'totalCost', data.now_cost);

            this.airConditioners[index].isOn = response.data.is_on;
            this.airConditioners[index].is_ava = response.data.is_ava;
            this.airConditioners[index].mode = this.mapAcModel(response.data.mode);
            this.airConditioners[index].targetTemp = response.data.tar_temp;
            this.airConditioners[index].currentTemp = response.data.cur_temp;
            this.airConditioners[index].fanSpeed = this.mapFanSpeed(response.data.wind);
            this.airConditioners[index].totalCost = response.data.now_cost;
            
            console.log('response.data:', response.data);
          }
        })
        .catch(error => {
          console.error('Error updating air conditioner info:', error);  
        });
    },

    mapFanSpeed(windValue) {
      // 将数字风速映射为字符串
      const speedMap = { 1: '低速', 2: '中速', 3: '高速' };
      return speedMap[windValue] || '中速'; 
    },

    mapAcModel(mode) {
    const modeMap = {
      'hot': '制热',
      'cold': '制冷'
    };
    return modeMap[mode] || '制冷'; // 默认为 'cold'
    },

    sendRate(){
      
      this.$axios.post('/set_ratio', {  low_rate: this.form2.speed1, mid_rate: this.form2.speed2, high_rate: this.form2.speed3, cost_rate: this.form2.cost_rate })
        .then(response => {
          console.log('success', response.data);
        })
        .catch(error => {
          console.error('Error updating air conditioner info:', error);  
        });
      this.$refs.form.validate((valid) => {
        if (valid) {
          // 表单验证通过
          this.dialogFormVisible2 = false; // 关闭对话框
          console.log('form2验证成功！');
        } else {
          // 表单验证失败
          console.log('form2验证失败！');
        }
      });

    },

    startRun(){
      this.$axios.post('http://127.0.0.1:5001/start')
        .then(response => {
          console.log('success start', response.data);
        })
        .catch(error => {
          console.error('Error updating air conditioner info:', error);  
        });
    },

    openControl(){
      this.$axios.get('/open_control')
        .then(response => {
          console.log('success start', response.data);
        })
        .catch(error => {
          console.error('Error updating air conditioner info:', error);  
        });
    },

},


};
</script>


<style scoped>


.container {
  display: flex;
  flex-direction: column;
  height: auto;
  align-items: center; /* 这会使 .content 子容器水平居中 */
  width: 104%;
}

.content {
  flex: 1;
  overflow: auto;
  padding: 20px;
}

.info-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  /* cursor: pointer; */
}

.info-cards .el-col {
  flex: 0 0 calc(20% - 10px); /* 5个col，每个col占20%，减去间距 */
  margin-bottom: 20px;
  padding: 10px; /* 根据需要添加内间距 */
}

.info-cards .el-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: .3s box-shadow;
  /* height: 100%; */
  height: auto;
  
}

.el-card{
  text-align: left;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1); 
}

.el-icon-setting {
  float: right; /* 图标放置在右上角 */
  cursor: pointer;
  transition: transform 0.3s ease; /* 添加动画效果，使图标在悬停时变化 */
  /* 根据需要调整图标大小 */
  font-size: 20px; /* 调整字体大小来改变图标大小 */
  margin-top: 5px;
  /* margin-right: 15px; */
}
.el-icon-setting:hover {
  transform: scale(1.05) translateY(-1.5px); /* 鼠标悬停时图标放大 */

}


.info-cards .el-card:hover {
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.i{
  justify-content: center;
  align-items: center;
}

.status-tag, h4 {
  text-align: left;
  margin-bottom: 10px;
}

/* 当屏幕尺寸小于某个值时隐藏某些信息 */
/* @media screen and (min-height: 1200px) {
  .info-cards h4:not(:first-child) { /* 除了第一个h3，其他隐藏 */
    /* display: none; */
  /* }
} */ 

.pagination {
  margin-top: 2vh;
  margin-right: 40px;
  display: flex;
  justify-content: center;

}

.setting1{
  /* margin-top: 2vh; */
  margin-left: 40px;
  float: left;
  /* justify-content: flex-start; */
}
.setting2{
  /* margin-top: 2vh; */
  margin-left: 40px;
  float: left;
  /* background-color: #42b983; */
  /* justify-content: flex-start; */
}
.testbutton1{
  background-color: #66b1ff;
}

.testbutton2{
  background-color: #67be97;
}
.switch{
  /* margin-top: 2vh; */
  margin-right: 40px;
  float: right;
}

.moneyinput{
  width: 50%;
}
</style>
