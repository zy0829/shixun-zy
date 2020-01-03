<template>
    <div>
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
        <el-table :data="customers">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>            
            <el-table-column prop="telephone" label="电话号码"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpadteHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!-- <el-pagination
           layout="prev, pager, next"
           :total="50">
        </el-pagination> -->
        <el-dialog
         :title="title"
         :visible.sync="visible"
         width="60%">
        --{{form}}
        <el-form :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item label="密码">
                <el-input  type="password" v-model="form.password "></el-input>
            </el-form-item>
            <el-form-item label="真实姓名">
                <el-input v-model="form.realname"></el-input>
            </el-form-item>
            <el-form-item label="手机号">
                <el-input v-model="form.telephone"></el-input>
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
    //用于存放网页需要调用的方法
    methods:{
        loadData(){
             let url="http://localhost:6677/customer/findAll"    
             request.get(url).then((response)=>{
             this.customers=response.data;
    })    
        },
        submitHandler(){
        //通过request与后台进行交互，并携带参数
           let url="http://localhost:6677/customer/saveOrUpdate"
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
           this.title="录入顾客信息"
           this.form={
               type:"customer"
           }
        },
        closeModalHandler(){
           this.visible=false;
        },
        toUpadteHandler(row){
            this.form=row;
            this.visible=true;
            this.title="修改顾客信息";
        },
        toDeleteHandler(id){
            //确认
            this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            let url="http://localhost:6677/customer/deleteById"
            request.get(url+"?id="+id).then((response)=>{
            this.loadData();
            this.$message({
            type: 'success',
            message: response.message
          });
            })               
        }) 
         }       
    },
    //用于存放数据
    data(){
        return{
            visible:false,
            title:"录入顾客信息",
            customers:[],
            form:{
                type:"customer"
            }

        }
    },
    created(){
        //vue实例创建完毕
    this.loadData();
    }
}
</script>

<style scoped>

</style>
