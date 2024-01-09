<template>
    <div class="room-information">
      <div class="actions">
        <el-button type="primary" @click="addRoom()" class="rbutton">入住房间</el-button>
        <el-button type="danger" @click="checkoutRoom()" class="rbutton">签出房间</el-button>
        <!-- <el-button type="info" @click="fetchRoom()" class="gbutton">生成详单</el-button> -->
      </div>
  
      <el-dialog :title="formTitle" :visible.sync="dialogFormVisible">
        <el-form :model="form_1" :rules="rules_1">
          <div v-if="formType === 'add'">
            <el-form-item label="房间号码" :label-width="formLabelWidth" prop="availabelRoom">
              <el-select v-model="form_1.availableRoom" placeholder="请选择房间号码">
                <el-option v-for="number in availableRooms" :key="number" :label="number" :value="number"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="姓名" :label-width="formLabelWidth" prop="userName">
              <el-input v-model="form_1.userName" placeholder="请输入客户姓名"></el-input>
            </el-form-item>
            <el-form-item label="身份证号码" :label-width="formLabelWidth" prop="idCard">
              <el-input v-model="form_1.idCard" placeholder="请输入客户身份证件号"></el-input>
            </el-form-item>
          </div>
  
          <div v-if="formType === 'delete'">
            <el-form-item label="房间号码" :label-width="formLabelWidth" prop="occupiedRoom">
              <el-select v-model="form_2.occupiedRoom" placeholder="请选择房间号码">
                <el-option v-for="number in occupiedRooms" :key="number" :label="number" :value="number"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="姓名" :label-width="formLabelWidth" prop="userName">
              <el-input v-model="form_2.userName" placeholder="请输入客户姓名"></el-input>
            </el-form-item>
            <el-form-item label="身份证号码" :label-width="formLabelWidth" prop="idCard">
              <el-input v-model="form_2.idCard" placeholder="请输入客户身份证件号"></el-input>
            </el-form-item>
          </div>
  
          <div v-if="formType === 'details'">
            <el-form-item label="房间号码" :label-width="formLabelWidth" prop="checkoutRoom">
              <el-select v-model="form_3.checkoutRoom" placeholder="请选择房间号码">
                <el-option v-for="number in checkoutRooms" :key="number" :label="number" :value="number"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="姓名" :label-width="formLabelWidth" prop="userName">
              <!-- <el-select v-model="form_3.userName" placeholder="请选择客户姓名">
                <el-option v-for="name in checkoutNames" :key="name" :label="name" :value="name"></el-option>
              </el-select> -->
              <el-input v-model="form_3.userName" placeholder="请输入客户姓名"></el-input>
            </el-form-item>
            <el-form-item label="身份证号码" :label-width="formLabelWidth" prop="idCard">
              <!-- <el-select v-model="form_3.idCard" placeholder="请选择客户身份证件号">
                <el-option v-for="number in checkoutIdCards" :key="number" :label="number" :value="number"></el-option>
              </el-select> -->
              <el-input v-model="form_3.idCard" placeholder="请输入客户身份证件号"></el-input>
            </el-form-item>
  
          </div>
  
  
        </el-form>
  
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取消</el-button>
          <!-- <el-button type="primary" @click="handleSubmit()">确认</el-button> -->
          <el-button v-if="formType === 'add'" type = "primary" @click="dialogFormVisible = false; handleFormSubmit()">确定</el-button>
          <el-button v-if="formType === 'delete'" type = "primary" @click="dialogFormVisible = false; handleFormSubmit(); billVisible = true">确定</el-button>
          <el-button v-if="formType === 'details'" type = "primary" @click="dialogFormVisible = false; handleFormSubmit(); detailsVisible = true">确定</el-button> 
        </div>              <!--对话dialog消失，详单生成-->>
      </el-dialog>
  
      <el-dialog :visible.sync="billVisible" title="账单详情">
        <div>
          <p>房间号码: {{ billData.room_id }}</p>
          <p>客户姓名: {{ billData.user_name }}</p>
          <p>身份证号: {{ billData.id_card }}</p>
          <p>入住时间: {{ billData.in_time }}</p>
          <p>退房时间: {{ billData.out_time }}</p>
          <p>当前费用: {{ billData.now_cost }}</p>
        </div>
      </el-dialog>

      <!-- <el-dialog :visible.sync="detailsVisible" title="详单">
        <div>
          <p>房间号码: {{ specsList.room_id }}</p>
            <p>请求时间: {{ specsList.request_time }}</p>
            <p>服务开始时间: {{ specsList.serve_start }}</p>
            <p>服务结束时间: {{ specsList.serve_end }}</p>
            <p>服务总时长: {{ specsList.total_serve }}</p>
            <p>风速: {{ specsList.wind }}</p>
            <p>当前费用: {{ specsList.now_cost }}</p>
            <p>费率: {{ specsList.cost_ratio }}</p>

        </div>
      </el-dialog> -->
      <el-dialog title="详单信息" :visible.sync="detailsVisible" width="70%">
        <el-table  :data="specsList" style="width: 100%">
          <el-table-column prop="room_id" label="房间号码"></el-table-column>
          <el-table-column prop="request_time" label="请求时间"></el-table-column>
          <el-table-column prop="serve_start" label="服务开始时间"></el-table-column>
          <el-table-column prop="serve_end" label="服务结束时间"></el-table-column>
          <el-table-column prop="total_serve" label="服务总时长"></el-table-column>
          <el-table-column prop="wind" label="风速"></el-table-column>
          <el-table-column prop="now_cost" label="当前费用"></el-table-column>
          <el-table-column prop="cost_ratio" label="费率"></el-table-column>
        </el-table>
      </el-dialog>
      
  
      <!-- <el-dialog :visible.sync="detailsVisible" title="详单" data="specList">   
        <el-table>
          <el-table-column prop="room_id" label="Room ID" width="120"></el-table-column>
          <el-table-column prop="request_time" label="请求时间" width="180"></el-table-column>
          <el-table-column prop="serve_start" label="服务开始时间" width="180"></el-table-column>
          <el-table-column prop="serve_end" label="服务结束时间" width="180"></el-table-column>
          <el-table-column prop="total_serve" label="服务总时长" width="150"></el-table-column>
          <el-table-column prop="wind" label="风速" width="100"></el-table-column>
          <el-table-column prop="now_cost" label="当前费用" width="120"></el-table-column>
          <el-table-column prop="cost_ratio" label="费率" width="120"></el-table-column>
        </el-table>
      </el-dialog> -->
  
      <div class="room-table">
        <el-table :data="operationRecords" border style="width: 100%" >
          <el-table-column prop="roomNumber" label="房间号" align="center"></el-table-column>
          <el-table-column prop="userName" label="客户姓名" align="center"></el-table-column>
          <el-table-column prop="idCard" label="身份证号码" align="center"></el-table-column>
          <el-table-column prop="operationType" label="操作类型" align="center"></el-table-column>
          <el-table-column prop="time" label="操作时间" align="center"></el-table-column>
        </el-table>
      </div>
      <el-button type="info" @click="fetchRoom()" class="gbutton">生成详单</el-button>
    </div>
  </template>
  
  
  <script>
  export default {
    data() {
      return {
        formLabelWidth:'',
        specsList: [],   // 记录详单
        // specsList: {
        //   room_id: '',
        //   request_time: '',
        //   serve_start: '',
        //   serve_end: '',
        //   total_serve: '',
        //   wind: '',
        //   now_cost: '',
        //   cost_ratio: ''
        // },

        detailsVisible: false,

        checkoutRooms: ['1', '2', '3', '4', '5'],
        checkoutNames: ['1', '2', '3', '4', '5'],
        checkoutIdCards: ['1', '2', '3', '4', '5'],
  
        billData: {   // 记录账单
          room_id: '',
          user_name: '',
          id_card: '',
          in_time: '',
          out_time: '',
          now_cost: ''
        },
        billVisible: false,
  
        availableRooms: ['1', '2', '3', '4', '5'], // 可用房间号
        occupiedRooms: ['1', '2', '3', '4', '5'],
        roomNumbers: ['1', '2', '3', '4', '5'], // 所有房间号
        operationRecords: [], // 操作记录
        dialogFormVisible: false,
  
        formType: '',
        formTitle: '',
        form_1: {
          // roomNumber: '',
          availableRoom: '',
          userName: '',
          idCard: ''
        },
        form_2: {
          occupiedRoom: '',
          userName: '',
          idCard: ''
        },
        form_3: {
          checkoutRoom: '',
          userName: '',
          idCard: '',
        },
        // rules_1: {
        //   availableRoom: [{ required: true, message: "请选择房间号码", trigger: "blur" }],
        //   userName: [{ required: true, message: "请输入客户姓名", trigger: "blur" }],
        //   idCard: [{ required: true, message: "请输入身份证号码", trigger: "blur" }]
        // },
        // rules_2: {
        //   occupiedRoom: [{ required: true, message: "请选择房间号码", trigger: "blur" }],
        //   userName: [{ required: true, message: "请输入客户姓名", trigger: "blur" }],
        //   idCard: [{ required: true, message: "请输入身份证号码", trigger: "blur" }]
        // },
        // rules_3: {
        //   checkoutRoom: [{ required: true, message: "请选择房间号码", trigger: "blur" }],
        //   userName: [{ required: true, message: "请输入客户姓名", trigger: "blur" }],
        //   idCard: [{ required: true, message: "请输入身份证号码", trigger: "blur" }]
        // }
      };
    },
    methods: {
      addRoom(){    // 在操作记录中添加add
        const record = {
          roomNumber: this.form_1.availableRoom,
          userName: this.form_1.userName,
          idCard: this.form_1.idCard,
          operationType: '新增'
        }
        this.formType = 'add',
        this.formTitle = '新建房间',
        this.dialogFormVisible = true
        // this.operationRecords.push(record);
      },
      checkoutRoom(){    // 在操作记录中添加checkout
        const record = {
          roomNumber: this.form_2.availableRoom,
          userName: this.form_2.userName,
          idCard: this.form_2.idCard,
          operationType: '登出'
        }
        this.formType = 'delete',
        this.formTitle = '登出房间',
        this.dialogFormVisible = true
        // this.operationRecords.push(record);
      },
      fetchRoom(){
        this.formType = 'details',
        this.formTitle = '生成详单',
        this.dialogFormVisible = true
      },

      handleFormSubmit() {
        let record = {};
        if (this.formType === 'add') {
          record = {
            roomNumber: this.form_1.availableRoom,
            userName: this.form_1.userName,
            idCard: this.form_1.idCard,
            operationType: '入住',
            time: new Date().toLocaleString()
          };
  
          // 将新建的availableRooms移到occupiedRooms和checkoutRooms中显示
          // 还需要把新建的userName和idCard移到checkoutNames和checkoutIdCards中显示
          let roomIndex = -1;
          for (let i = 0; i < this.availableRooms.length; i++) { 
            if (this.availableRooms[i] === this.form_1.availableRoom) { 
              roomIndex = i; 
              break; 
            }
          } 
          // if (roomIndex > -1) {   
          //   this.occupiedRooms.push(this.form_1.availableRoom);  
          //   this.checkoutRooms.push(this.form_1.availableRoom);
          //   this.checkoutNames.push(this.form_1.userName);
          //   this.checkoutIdCards.push(this.form_1.idCard);
          //   this.availableRooms.splice(roomIndex, 1);  // 从availableRooms中删除
          // }
  
          // 还需要把新建的userName和idCard移到checkoutNames和checkoutIdCards中显示
          // this.checkoutNames.push(this.form_1.userName);
          // this.checkoutIdCards.push(this.form_1.idCard); 
  
          //传入数据
          this.handleFormSubmit1(record.roomNumber,record.userName,record.idCard);
        } 
        
        else if (this.formType === 'delete') {
          record = {
            roomNumber: this.form_2.occupiedRoom,
            userName: this.form_2.userName,
            idCard: this.form_2.idCard,
            operationType: '登出',
            time: new Date().toLocaleString()
          };
  
          // 将登出的occupiedRooms移到availableRooms中显示
          let roomIndex = -1;
          for (let i = 0; i < this.occupiedRooms.length; i++) {
            if (this.occupiedRooms[i] === this.form_2.occupiedRoom) {
              roomIndex = 1;
              break;
            }
          }
          // if (roomIndex > -1) {
          //   this.availableRooms.push(this.form_2.occupiedRoom);
          //   this.occupiedRooms.splice(roomIndex, 1);
          // }
  
          // 生成账单
          this.handleFormSubmit2(record.roomNumber,record.userName,record.idCard);  // billVisible如果在这里不能及时显现在页面上的话，可以在确认键部分直接添加
        }
  
        else if (this.formType === 'details') {
          // 对details的操作
        //   operationType = '生成详单';  // 尚未实现，也可以使用const record方法在操作表单中显示生成详单操作
          this.fetchRoomDetails(this.form_3.checkoutRoom,this.form_3.userName,this.form_3.idCard);
        }
  
        // 操作记录中显示
        this.operationRecords.push(record);
        this.dialogFormVisible = false;
        this.resetForm(); // 重置表单
      },
  
      handleFormSubmit1(room_id,user_name,id_card) {  // 传入数据，响应为0

        const info = {
        room_id:room_id,
        user_name:user_name,
        id_card:id_card,
        };

        this.$axios.post('/check_in', info)
        .then(response => {

          console.log('</check_in> successfully!');
          console.log(response.data);
        })
        .catch(error => {
          console.error('Error </check_in>', error);
        });
      },
      handleFormSubmit2(room_id,user_name,id_card) {   // 传入数据，得到响应，

        const info = {
        room_id:room_id,
        user_name:user_name,
        id_card:id_card,
        };

        this.$axios.post('/make_bill', info)
        .then(response => {
            
            this.billData = {
            ...response.data,
            ...info,
            };

          this.billVisible = true;
          console.log('</make_bill> successfully!');
          console.log(response.data);
        })
        .catch(error => {
          console.error('Error </make_bill>', error);
        });
        
      },
      resetForm() {
        // this.operationRecords = {
        //   roomNumber: '',
        //   userName: '',
        //   idCard: '',
        //   operationType: ''
        // };
        this.form_1 = {
          availableRoom: '',
          userName: '',
          idCard:''
        };
        this.form_2 = {
          occupiedRoom: '',
          userName: '',
          idCard: ''
        };
        this.form_3 = {
          checkoutRoom: '',
          userName: '',
          idCard: ''
        };
      },
      fetchRoomDetails(room_id,user_name,id_card) {  ///make_specifi
        // TODO: 生成详单的逻辑
        const info = {
        room_id:room_id,
        user_name:user_name,
        id_card:id_card,
        };

        this.$axios.post('/make_specifi', info)
        .then(response => {
            
            this.specsList = response.data.specs_list,
            
            this.detailsVisible = true;
          console.log('</make_specifi> successfully!');
          console.log(response.data);
        })
        .catch(error => {
          console.error('Error </make_specifi>', error);
        });

        this.formType = 'details',
        this.formTitle = '生成详单',
        this.dialogFormVisible = true;
       
      },
    }
  };
  </script>
  
  <style lang="less" scoped>
    .room-table {
      margin-top: 25px;
    }

    .room-information{
      margin-top: 10px;
      margin-left:40px;
      margin-right:40px;
    }

    .rbutton{
      margin-right: 10px;
      margin-left: 15px;
    }

    .gbutton{
      //从右边开始
      float: right;
      margin: 10px;
      margin-right: 20px;
    }

  </style>
  