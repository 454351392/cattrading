<template>
  <div class="usermanagement">
    <h2>商品管理</h2>
    <goods-search @myevent="searchUsers"></goods-search>
    <div class="mytable">
      <el-table :data="tableData" border style="width: 80%" height="700">
        <el-table-column type="index" width="50" label="序号"></el-table-column>
        <el-table-column prop="id" label="id" width="60"></el-table-column>
        <el-table-column prop="class1" label="品种" width="90"></el-table-column>
        <el-table-column prop="sexs" label="性别" width="80"></el-table-column>
        <el-table-column prop="price" label="价格" width="80"></el-table-column>
        <el-table-column prop="username" label="售卖人" width="100"></el-table-column>
        <el-table-column prop="num" label="数量" width="100"></el-table-column>
        <el-table-column prop="shop" label="店铺" width="100"></el-table-column>
        <el-table-column prop="tel" label="联系电话" width="110"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-popover placement="left" width="500" trigger="click">
              <h4 class="goodinfo">详情</h4>
              <div class="catinfo">
                <div>
                  <span class="ass">id:</span>
                  <span>{{goodinfo.id}}</span>
                </div>
                <div>
                  <span class="ass">品种:</span>
                  <span>{{goodinfo.class1}}</span>
                </div>
                <div>
                  <span  class="ass mypic1">图片:</span>
                  <img class="mypic2" :src="goodinfo.pic" alt="">
                  <!-- <span>{{goodinfo.pic}}</span> -->
                </div>
                <div>
                  <span class="ass">性别:</span>
                  <span>{{goodinfo.sexs}}</span>
                </div>
                <div>
                  <span class="ass">年龄:</span>
                  <span>{{goodinfo.age}}</span>
                </div>
                <div>
                  <span class="ass">价格:</span>
                  <span>{{goodinfo.price}}</span>
                </div>
                <div>
                  <span class="ass">数量:</span>
                  <span>{{goodinfo.num}}</span>
                </div>
                <div>
                  <span class="ass">售卖人:</span>
                  <span>{{goodinfo.username}}</span>
                </div>
                <div>
                  <span class="ass">店铺:</span>
                  <span>{{goodinfo.shop}}</span>
                </div>
                <div>
                  <span class="ass">简介:</span>
                  <span>{{goodinfo.title1}}</span>
                </div>
                <div>
                  <span class="ass">详情:</span>
                  <span>{{goodinfo.info}}</span>
                </div>
              </div>
              <el-button
                size="mini"
                slot="reference"
                type="primary"
                @click="handleEdit(scope.$index, scope.row)"
              >详情</el-button>
              <!-- <el-button slot="reference">click 激活</el-button> -->
            </el-popover>
            <!-- <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">详情</el-button> -->
            <el-button
              size="mini"
              class="mydelet"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

  <script>
import goodssearch from "@/components/admin/goodssearch.vue";
export default {
  data() {
    return {
      tableData: [],
      goodinfo: {}
    };
  },
  components: {
    "goods-search": goodssearch
  },
  created() {
    // this.axios
    //   .get("/goodszhanshi")
    //   .then(res => {
    //     // console.log(response);
    //     // this.employee = response.data;
    //     //拿到数据之后去触发自定义事件，不允许大驼峰式写法
    //     console.log(res.data);
    //     this.tableData = res.data;
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
  },
  methods: {
    searchUsers(data) {
      this.tableData = data;
    },
    handleEdit(index, row) {
      this.goodinfo = row;
    },
    handleDelete(index, row) {
      console.log(index, row);
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.axios
            .post("/deletegoogd", row)
            .then(res => {
              // console.log(response);
              // this.employee = response.data;
              //拿到数据之后去触发自定义事件，不允许大驼峰式写法
              console.log(res.data);
              _this.tableData.splice(index, 1);
            })
            .catch(function(error) {
              console.log(error);
            });
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    }
  }
};
</script>

<style scoped>
.usermanagement {
  width: 70%;
  position: relative;
  top: 120px;
  left: 500px;
}

.mydelet {
  margin-left: 17px;
}

.goodinfo {
  text-align: center;
  font-size: 24px;
}

.catinfo {
  margin: 0px auto;
}

.catinfo div {
  margin: 10px auto;
  border-bottom: 1px solid rgb(209, 206, 206);
}

.ass {
  font-weight: bold;
  font-size: 22px;
}

span {
  font-size: 17px;
}

.mypic1{
  display: block;
}

.mypic2{
  width: 200px;
  height: 200px;
}
</style>

