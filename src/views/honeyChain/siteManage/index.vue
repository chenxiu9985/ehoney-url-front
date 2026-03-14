<template>
  <PageContent v-loading="loading">
    <div slot="form" class="page-form">
      <form-item label="查询内容">
        <el-input placeholder="请输入查询内容" v-model="searchParams.Payload" clearable @keyup.enter.native="search"></el-input>
      </form-item>
      <div class="btn-box">
        <el-button size="small" type="primary" @click="search">查询</el-button>
        <el-button size="small" @click="handleCreate">新建</el-button>
      </div>
    </div>
    <div class="page-main">
      <ListLayout>
        <el-table slot="list" :data="tableData" width="100%" height="100%" :border="false">
          <el-table-column label="场景名称" prop="SceneName"></el-table-column>
          <el-table-column label="蜜链域名" prop="Domain"></el-table-column>
          <el-table-column label="蜜链数量" prop="ChainCount"></el-table-column>
          <el-table-column label="状态" prop="Status">
            <template v-slot="{row}">
              <span :class="getStatusClass(row.Status)">{{ getStatusText(row.Status) }}</span>
            </template>
          </el-table-column>
          <el-table-column label="创建时间" prop="CreateTime"></el-table-column>
          <el-table-column label="操作" width="200px">
            <template v-slot="{row}">
              <el-button
                :type="row.Status === 1 ? 'danger' : 'success'"
                size="mini"
                @click="toggleStatus(row)"
              >
                {{ row.Status === 1 ? '停用' : '启动' }}
              </el-button>
              <el-button
                type="danger"
                size="mini"
                @click="handleDelete(row)"
              >
                删除
              </el-button>
            </template>
          </el-table-column>
          <div class="table-empty" slot="empty" style="width: 100%;">
            <img src="@/assets/images/empty.png" alt="" width="305" height="159" />
          </div>
        </el-table>
        <Pagination slot="pagination" @size-change="sizeChange" @current-change="getTableData" :total="pagination.totalCount" :pageSize="pagination.size" :current-page.sync="pagination.page" />
      </ListLayout>
    </div>
    <!-- 新建蜜链生成任务弹窗 -->
    <AddTaskDialog v-if="isShowAddDialog" @close="isShowAddDialog = false" @save="search" />
  </PageContent>
</template>

<script>
import AddTaskDialog from './AddTaskDialog.vue';
import pageList from '@/mixins/pageList';

export default {
  components: { AddTaskDialog },

  mixins: [pageList],

  data () {
    return {
      searchUrl: this.$apis.honeyChainSiteSearch,
      apiUrl: this.$apis.honeyChainSiteCreate,
      isShowAddDialog: false
    }
  },

  created () {
    this.search();
  },

  methods: {
    /**
     * 获取列表数据（使用mock数据）
     */
    getTableData (loading = true) {
      this.loading = loading;
      
      // Mock数据
      const mockData = {
        data: {
          List: [
            {
              ID: '1',
              SceneName: '电商场景',
              Domain: 'fake-shop-1.com',
              ChainCount: 10,
              Status: 1,
              CreateTime: '2024-01-15 10:30:00'
            },
            {
              ID: '2',
              SceneName: '金融场景',
              Domain: 'fake-bank-2.com',
              ChainCount: 15,
              Status: 2,
              CreateTime: '2024-01-14 14:20:00'
            },
            {
              ID: '3',
              SceneName: '政务场景',
              Domain: 'fake-gov-3.com',
              ChainCount: 8,
              Status: 1,
              CreateTime: '2024-01-13 09:15:00'
            },
            {
              ID: '4',
              SceneName: '医疗场景',
              Domain: 'fake-hospital-4.com',
              ChainCount: 12,
              Status: 1,
              CreateTime: '2024-01-12 16:45:00'
            },
            {
              ID: '5',
              SceneName: '教育场景',
              Domain: 'fake-edu-5.com',
              ChainCount: 6,
              Status: 2,
              CreateTime: '2024-01-11 11:30:00'
            }
          ],
          Count: 5
        }
      };

      // 模拟异步请求
      setTimeout(() => {
        this.loading = false;
        this.tableData = mockData.data.List || [];
        this.pagination.totalCount = mockData.data.Count;
      }, 300);
    },

    /**
     * 获取状态文本
     */
    getStatusText (status) {
      const statusMap = {
        1: '运行中',
        2: '已停用'
      };
      return statusMap[status] || '未知';
    },

    /**
     * 获取状态样式类
     */
    getStatusClass (status) {
      return status === 1 ? 'green--text' : 'grey--text';
    },

    /**
     * 切换状态（启动/停用）
     */
    toggleStatus (row) {
      const actionText = row.Status === 1 ? '停用' : '启动';
      this.$confirm(`确认${actionText}该站点?`, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // Mock状态切换
        row.Status = row.Status === 1 ? 2 : 1;
        this.$message.success(`${actionText}成功！`);
      }).catch(() => {});
    },

    /**
     * 新建按钮点击事件
     */
    handleCreate () {
      this.isShowAddDialog = true;
    },

    /**
     * 删除站点
     */
    handleDelete (row) {
      this.$confirm('确认删除该站点?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // Mock删除操作
        const index = this.tableData.findIndex(item => item.ID === row.ID);
        if (index !== -1) {
          this.tableData.splice(index, 1);
          this.pagination.totalCount--;
        }
        this.$message.success('删除成功！');
      }).catch(() => {});
    }
  }
}
</script>
