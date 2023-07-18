<template>
  <div class="table-box">

    <!-- 标题 -->
    <div class="title">
      <h2>最简单的crud Demo</h2>
    </div>

    <!-- query 搜索区 -->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" />
      <div class="btn-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
        <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length > 0 ">删除多选</el-button>
      </div>
     
    </div>

    <!-- table -->
    <el-table 
      border 
      ref="multipleTableRef"
      :data="tableData"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
    <el-table-column type="selection" width="55" />
    <el-table-column prop="name" label="姓名" width="120" />
    <el-table-column prop="email" label="邮箱" width="120" />
    <el-table-column prop="phone" label="电话" width="120" />
    <el-table-column prop="state" label="状态" width="120" />
    <el-table-column prop="address" label="地址" width="300" />

    <el-table-column fixed="right" label="操作" width="120">
      <template #default="scope">
        <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color:#F56C60">删除</el-button>
        <el-button link type="primary" size="small">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>

  <!-- dialog弹窗 -->
  <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add' ? '新增' : '编辑'">
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="80">
        <el-input v-model="tableForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="邮箱" :label-width="80">
        <el-input v-model="tableForm.email" autocomplete="off" />
      </el-form-item>
      <el-form-item label="电话" :label-width="80">
        <el-input v-model="tableForm.phone" autocomplete="off" />
      </el-form-item>
      <el-form-item label="状态" :label-width="80">
        <el-input v-model="tableForm.state" autocomplete="off" />
      </el-form-item>
      <el-form-item label="地址" :label-width="80">
        <el-input v-model="tableForm.address" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>



  </div>
</template>


<script setup>
import { ref } from 'vue';

  /*数据*/
let queryInput = ref("")
let tableData = ref([
 {
    id:"1",
    name: 'Tom',
    email:"123@qq.com",
    phone:"13814145656",
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"2",
    name: 'Tom',
    email:"123@qq.com",
    phone:"13814145656",
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"3",
    name: 'Tom',
    email:"123@qq.com",
    phone:"13814145656",
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"4",
    name: 'Tom',
    email:"123@qq.com",
    phone:"13814145656",
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
])
let multipleSelection = ref([])
let dialogFormVisible = ref(false)
let tableForm = ref({
  name:"张三",
  email:"123@qq.com",
  phone:"19962677232",
  state:"在职",
  address:"广东省"
})
let dialogType = ref('add')


  /*方法*/
  
//删除单条数据
const handleRowDel = ({id}) => {
  console.log(id)
  //1.通过id获取到条目对应的 索引值
  let index = tableData.value.findIndex(item => item.id === id)
  // console.log(index);
  //2.通过索引值进行删除对应条目
  tableData.value.splice(index,1)
}

const handleDelList = () => {
  multipleSelection.value.forEach(id => {
    handleRowDel({id})
  })
  multipleSelection.value = []
}



// 选中
const handleSelectionChange = (val) => {
  // multipleSelection.value = val
  // console.log(val);
  multipleSelection.value = []
  val.forEach(item => {
  multipleSelection.value.push(item.id)
  })

  console.log(multipleSelection.value);

}




// 新增
const handleAdd = () => {
  dialogFormVisible.value = true
  tableForm.value = {}
}
// 确认
const dialogConfirm = () => {
  dialogFormVisible.value = false
  //1.拿到数据

  //2.添加到table
  tableData.value.push({
    id:(tableData.value.length + 1).toString(),
    ...tableForm.value
  })
  console.log(tableData.value);
}

</script>


<style scoped>
.table-box{
  margin: 200px auto;
  width: 800px;
}

.title{
  text-align: center;
}

.query-box{
  display: flex;
  justify-content: space-between; /* 将容器内的子元素在主轴上（横向）均匀分布，两端对齐 */
  margin-bottom: 20px;
}
.query-input{
  width: 200px;
}
</style>
