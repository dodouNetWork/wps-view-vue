<template>
  <div class="dbFile">
    <div id="nav">
      <h2>
        <router-link to="/webFile">webFile</router-link>
        &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;
        <router-link to="/dbFile">dbFile</router-link>
      </h2>
    </div>
    <el-table
            :data="tableData"
            size="mini"
            align="center"
            style="width: 100%">
      <el-table-column
              prop="name"
              label="文件名"
              align="center"
              header-align="center"
              width="180">
      </el-table-column>
      <el-table-column
              prop="version"
              label="版本号"
              align="center"
              header-align="center"
              width="70">
      </el-table-column>
      <el-table-column
              prop="size"
              label="文件大小"
              align="center"
              header-align="center"
              width="90">
      </el-table-column>
      <el-table-column
                prop="creator"
                label="创建者"
                align="center"
                header-align="center"
                width="70">
      </el-table-column>
      <el-table-column
                prop="createTime"
                label="创建时间"
                align="center"
                header-align="center"
                width="120">
      </el-table-column>
      <el-table-column
              prop="modifier"
              label="修改者"
              align="center"
              header-align="center"
              width="70">
      </el-table-column>
      <el-table-column
              prop="modifyTime"
              label="修改时间"
              align="center"
              header-align="center"
              width="120">
      </el-table-column>
      <el-table-column label="操作"
                       align="center"
                       header-align="center"
                       width="260">
        <template slot-scope="scope">
          <el-button round
                  size="mini"
                  @click="handleEdit(scope.row.id,scope.row.userId)">编辑/预览</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  export default {
    name: "dbFile",
    data() {
      return {
        tableData: []
      }
    },
    created(){
      this.getFileList();
    },
    methods: {
      getFileList(){
        this.axios.get('/v1/api/file/getFileList').then((res) => {
          if (res.data) {
            this.tableData = res.data.data;
          }else {
            this.showErrMeg('请求错误！');
          }
        }).catch(()=>{
          this.showErrMeg('请求错误！');
        });
      },
      getViewUrlDbPath(fileId,userId){
        this.axios.get('/v1/api/file/getViewUrlDbPath?fileId=' + fileId + "&userId=" + userId).then((res) => {
          if (res.data) {
            let r = res.data.data;

            // 跳转 使用sessionStorage，避免关键信息在ip中暴露
            // 使用push会停留当前页面，故不采纳
            // params 传递参数，子组件无法渲染iframe组件，故不采纳
            sessionStorage.wpsUrl = r.wpsUrl;
            sessionStorage.token = r.token;
            const jump = this.$router.resolve({name: 'viewFile'});
            window.open(jump.href,'_blank');
          }else {
            this.showErrMeg('请求错误！');
          }
        }).catch(()=>{
          this.showErrMeg('请求错误！');
        });
      },
      handleEdit(fileId,userId){
        this.getViewUrlDbPath(fileId,userId);
      }
    }
  }
</script>

<style scoped>
</style>
