<template>
    <div>
        <!-- 按钮 -->
        员工管理<br>
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>
        <!-- 表格 -->
        <el-table :data="employees">
            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="username" label="用户名"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column width="120" prop="telephone" label="联系方式"></el-table-column>
            <el-table-column width="200" prop="idCard" label="身份证号"></el-table-column>
            <el-table-column width="200" prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column fixed="right" label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row.id)">修改</a>
                    </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <!-- 模态框 -->
        <el-dialog
            :title="title"
            :visible.sync="visible"
            width="60%">
            测试：{{form}}
            <el-form label-width="80px" :model="form">
                <el-form-item  label="用户名">
                    <el-input v-model="form.username">
                    </el-input>
                </el-form-item>
                <el-form-item label="密码" >
                    <el-input type="password" v-model="form.password">

                    </el-input>
                </el-form-item>
                <el-form-item label="姓名" >
                    <el-input v-model="form.realname">

                    </el-input>
                    </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="form.gender">
                     <el-radio label="男">男</el-radio>
                     <el-radio label="女">女</el-radio>
                </el-radio-group>
                </el-form-item>

                
                <el-form-item label="手机号">
                    <el-input v-model="form.telephone">

                    </el-input>
                </el-form-item>
                <el-form-item label="身份证号">
                    <el-input v-model="form.idCard">

                    </el-input>
                </el-form-item>
                <el-form-item label="银行卡号">
                    <el-input v-model="form.bankCard">

                    </el-input>
                </el-form-item>
            </el-form>

            <span slot="footer" class="dialog-footer">
                <el-button size="small" @click="closeModalHandler">取 消</el-button>
                <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
        
    </div>
</template>

<script>
import request from '@/utils/request'//自定义库
import querystring from 'querystring'//系统库
export default {
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
                type:"waiter"
            }
        }
    },
    created(){

        //在页面加载出来的时候加载数据
        this.loadData();
    },
    methods:{
        submitHandler(){
            let url="http://localhost:6677/waiter/saveOrUpdate"
            //前端向后台发送请求，完成数据的保存操作
           // this.closeModalHandler();
                request({
                    url,
                    method:"POST",
                    headers:{
                        "Content-Type":"application/x-www-form-urlencoded"
                    },
                     data: querystring.stringify(this.form)
                }).then((Response)=>{
                    this.closeModalHandler();
                    this.loadData();
                    this.$message({type:"success",message:response.message})
                })
            
        },
        loadData(){
            let url ="http://localhost:6677/waiter/findAll";
            request .get(url).then((responsse)=>{
                //箭头函数中的this指向外部函数中的this
                this.employees=responsse.data;
            })

        },
        toAddHandler(row){
            this.title="录入员工信息"
            this.visible=true;
        },
        closeModalHandler(){
            this.visible=false;
        },
        toDeleteHandler(id){
             this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
        toUpdateHandler(){
            this.title="修改员工信息";
            this.visible=true;

        },
    }
}
</script>
<style scoped>

</style>