<template>
  <div class="flex-container">
    <div class="center">
      <div class = "title"> 
        Stop Employee Role
      </div>
      <div class="stop-form" >
        <el-form ref="form" :model="form" label-width="100px">
       <el-form-item>
       <el-select v-model="form.employeeID">
            <el-option
              v-for="item in form.employeeList"
              :key="item.perID"
              :label="item.perID"
              :value="item.perID">
            </el-option>
          </el-select>
       </el-form-item>
        </el-form>
      </div>
      <div class="buttons-item" >
        <el-button class="btn" type="primary" @click="back">Back</el-button>
        <el-button class="btn" type="primary" @click="StopEmployeeRole">Confirm</el-button>
      </div>
      <div class = "title"> 
        Stop Customer Role
      </div>
      <div class="stop-form" >
        <el-form ref="form" :model="form" label-width="100px">
       <el-form-item>
       <el-select v-model="form.customerID">
            <el-option
              v-for="item in form.customerList"
              :key="item.perID"
              :label="item.perID"
              :value="item.perID">
            </el-option>
          </el-select>
       </el-form-item>
        </el-form>
      </div>
      <div class="buttons-item" >
        <el-button class="btn" type="primary" @click="back">Back</el-button>
        <el-button class="btn" type="primary" @click="StopCustomerRole">Confirm</el-button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
    data(){
        return{
            form: {
          customerID: '',
          employeeID: '',
          employeeList: [],
          customerList: []
          }
        }
    },
    created(){
      this.getEmployeeList();
      this.getCustomerList();
    },
    methods: {
      back: function(){
        this.$router.push('/manageusers')
      },   
      onCreateEmployee() {
        console.log('Create employee!');
      },
      onCancelEmployee(){
          console.log('Stop employee!');
      },
      onCreateCustomer(){
        console.log('Create customer!');
      },
      onCancelCustomer(){
        console.log('Stop customer!')
      },
      getEmployeeList(){
        this.axios({
          method:"get",
          url: "http://localhost:3000/employeeList", // 接口地址
        }).then(response=>{
          let result = response.data
          this.form.employeeList=result
          console.log(result)
        }).catch(error => console.log(error, "error")); // 失败的返回
      },
      getCustomerList(){
        this.axios({
          method:"get",
          url: "http://localhost:3000/customerList", // 接口地址
        }).then(response=>{
          let result = response.data
          this.form.customerList=result
          console.log(result)
        }).catch(error => console.log(error, "error")); // 失败的返回
      },
      StopEmployeeRole(){
        if(this.form.employeeID){
        this.axios({
          method:"post",
          url: "http://localhost:3000/StopEmployeeRole", // 接口地址
          data:{
            PersonID: this.form.employeeID,
          }
          }).then(res=>{
          console.log("StopEmployeeRole",res)
          if(res.data.affectedRows != 0) {
            this.$message({
              message: `Sucessfully Stop ${this.form.employeeID}!`,
              type: 'success'
          });
          } else {
            this.$message({
              message: `Can not stop the employee Role!`,
              type: 'warning'
          });
          }
          }).catch(error => console.log(error, "error")); // 失败的返回
        }else {
           this.$message({
              message: `Select a Employee!`,
              type: 'warning'
          });
        }
      },

       StopCustomerRole(){
        if(this.form.customerID){
        this.axios({
          method:"post",
          url: "http://localhost:3000/StopCustomerRole", // 接口地址
          data:{
            PersonID: this.form.customerID,
          }
        }).then(res=>{
          console.log("StopEmployeeRole",res)
          if(res.data.affectedRows != 0) {
            console.log("stop")
            this.$message({
                message: `Sucessfully Stop ${this.form.customerID}!`,
                type: 'success'
            });
          } else {
            this.$message({
              message: `Can not stop the Customer Role!`,
              type: 'warning'
            });
          }
        }).catch(error => console.log(error, "error")); // 失败的返回
      } else {
        this.$message({
          message: `Select a Customer!`,
          type: 'warning'
        });
      }
    }
  }
}
</script>

<style scoped>
.flex-container {
  width: 100vw;
  height: 100vh;
  display: flex; 
  justify-content: center;
  align-items: center;
}

.center {
  display: flex;
  flex-direction: column;
  width: 500px;
  height: 500px;
  margin: auto;
  padding: 100px;
  border: 1px solid #409EFF;
  text-align: left;
}
.stop-form{
  margin-top: 30px;
  margin-left: -100px;
  align-self: center;
}
.buttons-item{
    align-self: center;
    margin:30px;
}
.btn{
  width: 150px;
  font-size: 15px;
}
.title{
  align-self: center;
  font-size: 25px;
}
.input-with-select .el-input-group__prepend {
background-color: #fff;}
</style>