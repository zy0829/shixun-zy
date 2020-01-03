<template>
    <div>
        <h1>产品管理</h1>
       <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
        <el-table :data="product">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="产品名称"></el-table-column>
            <el-table-column prop="price" label="价格"></el-table-column>
            <el-table-column prop="description"  label="描述"></el-table-column>
            <el-table-column prop="categoryId" label="所属产品"></el-table-column>
            <el-table-column fixed="right" label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpadteHandler(slot.row)">修改</a>
                    <a href="" @click.prevent="toUpadteHandler(slot.row)">详情</a>
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
            <el-form-item label="产品名称">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="价格">
                <el-input v-model="form.price"></el-input>
            </el-form-item>
            <el-form-item label="描述">
              <el-input v-model="form.description"></el-input>
            </el-form-item>
            <el-form-item label="所属栏目">
            <el-dropdown split-button type="primary" @click="handleClick" v-model="form.status">
                   请选择
                <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>12</el-dropdown-item>
                <el-dropdown-item>家具养护</el-dropdown-item>
                <el-dropdown-item>9357</el-dropdown-item>
                <el-dropdown-item>洗护服务</el-dropdown-item>
                <el-dropdown-item>生活急救箱</el-dropdown-item>
                <el-dropdown-item>yyy</el-dropdown-item>
                <el-dropdown-item>水果慢羊羊</el-dropdown-item>
            </el-dropdown-menu>
            </el-dropdown>
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
        let url="http://localhost:6677/product/findAll"    
             request.get(url).then((response)=>{
             this.product=response.data;})
        },
        submitHandler(){
          let url="http://localhost:6677/product/saveOrUpdate"
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
        this.title="添加产品信息";
       
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
        this.title="修改产品信息";
    }
    },
    data(){
        return{
            title:"添加产品",
            visible:false,
            product:[],
            form:{
                type:"product"
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