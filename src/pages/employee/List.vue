<template>
    <div>
       <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
        <el-table :data="employee">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone"  label="联系方式"></el-table-column>
            <el-table-column prop="idCard" label="身份证号"></el-table-column>
            <el-table-column prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column fixed="right" label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpadteHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination
           layout="prev, pager, next"
           :total="50">
        </el-pagination>
        <el-dialog
         :title="title"
         :visible.sync="visible"
         width="60%">
         {{form}}        
        <el-form  :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item label="密码">
                <el-input  type="password" v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item label="真实姓名">
                <el-input v-model="form.realname"></el-input>
            </el-form-item>
            <el-form-item label="性别">
                <el-radio-group v-model="form.gender">
                    <el-radio label="男">男</el-radio>
                    <el-radio label="女">女</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="电话号码">
                <el-input v-model="form.telephone"></el-input>
            </el-form-item>
            <el-form-item label="身份证号" >
                <el-input v-model="form.idCard" ></el-input>
            </el-form-item>
            <el-form-item label="银行卡号" >
                <el-input v-model="form.bankCard"></el-input>
            </el-form-item>
        </el-form> 
        <span slot="footer" class="dialog-footer">
        <el-button @click="closeModalHandler" >取 消</el-button>
        <el-button type="primary" @click="submitHandler">确 定</el-button>
        </span>
       </el-dialog>
    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    methods:{
        loadData(){
        let url="http://localhost:6677/waiter/findAll"    
             request.get(url).then((response)=>{
             this.employee=response.data;})
        },
        submitHandler(){
          let url="http://localhost:6677/waiter/saveOrUpdate"
           request({
               url,
               method:"POST",
               headers:{
                   "Content-Type":"application/x-www-form-urlencoded"
               },
               data:querystring.stringify(this.form)
           }).then((response)=>{
               //关闭模态框
               this.closeModalHandler();
               //提示消息
               this.loadData();
               this.$message({
                   type:"success",
                   message:response.message
               })
           })
        },
    toAddHandler(){
        this.visible=true;
        this.title="录入员工信息";
       
    },
      closeModalHandler(){
        this.visible=false;
    },
    toDeleteHandler(id){
        this.$confirm('此操作将永久删除该记录 , 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '编号为'+id+'，删除成功!'
          });
        })
    },
    toUpadteHandler(row){
        this.visible=true;
        this.title="修改员工信息";
    }
    },
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employee:[],
            form:{
                type:"waiter"
                }
        }
    },
    created(){
        this.loadData();
    }
}
</script>

<style scoped>

</style>