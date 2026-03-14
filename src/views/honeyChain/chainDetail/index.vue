<template>
  <PageContent v-loading="loading">
    <div class="page-header">
      <el-button size="small" icon="el-icon-arrow-left" @click="goBack">返回</el-button>
      <span class="page-title">蜜链详情 - {{ targetName }}</span>
    </div>
    <div class="page-main">
      <ListLayout>
        <el-table slot="list" :data="tableData" width="100%" height="100%" :border="false">
          <el-table-column label="序号" prop="Seq" width="80px"></el-table-column>
          <el-table-column label="蜜链URL" prop="ChainUrl"></el-table-column>
          <el-table-column label="相似度分数" prop="SimilarityScore" width="120px">
            <template v-slot="{row}">
              <el-tag :type="getScoreType(row.SimilarityScore)">{{ row.SimilarityScore }}</el-tag>
            </template>
          </el-table-column>
          <el-table-column label="访问次数" prop="VisitCount" width="100px"></el-table-column>
          <el-table-column label="最近访问时间" prop="LastVisitTime" width="180px"></el-table-column>
          <div class="table-empty" slot="empty" style="width: 100%;">
            <img src="@/assets/images/empty.png" alt="" width="305" height="159" />
          </div>
        </el-table>
        <Pagination slot="pagination" @size-change="sizeChange" @current-change="getTableData" :total="pagination.totalCount" :pageSize="pagination.size" :current-page.sync="pagination.page" />
      </ListLayout>
    </div>
  </PageContent>
</template>

<script>
import pageList from '@/mixins/pageList';

export default {
  mixins: [pageList],

  data () {
    return {
      targetName: '',
      targetId: ''
    }
  },

  created () {
    this.targetId = this.$route.query.targetId || '';
    this.targetName = this.$route.query.targetName || '未知目标';
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
              Seq: 1,
              ChainUrl: 'http://shop1.fake-shop.com/product/123',
              SimilarityScore: 95,
              VisitCount: 23,
              LastVisitTime: '2024-01-15 14:30:00'
            },
            {
              Seq: 2,
              ChainUrl: 'http://shop2.fake-shop.com/item/456',
              SimilarityScore: 88,
              VisitCount: 18,
              LastVisitTime: '2024-01-15 13:25:00'
            },
            {
              Seq: 3,
              ChainUrl: 'http://shop3.fake-shop.com/goods/789',
              SimilarityScore: 92,
              VisitCount: 15,
              LastVisitTime: '2024-01-15 12:10:00'
            },
            {
              Seq: 4,
              ChainUrl: 'http://shop4.fake-shop.com/product/abc',
              SimilarityScore: 76,
              VisitCount: 12,
              LastVisitTime: '2024-01-15 11:05:00'
            },
            {
              Seq: 5,
              ChainUrl: 'http://shop5.fake-shop.com/item/def',
              SimilarityScore: 83,
              VisitCount: 9,
              LastVisitTime: '2024-01-15 10:00:00'
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
     * 获取分数标签类型
     */
    getScoreType (score) {
      if (score >= 90) return 'success';
      if (score >= 80) return 'warning';
      return 'info';
    },

    /**
     * 返回上一页
     */
    goBack () {
      this.$router.back();
    }
  }
}
</script>

<style scoped lang="less">
.page-header {
  display: flex;
  align-items: center;
  padding: 10px 15px;
  background: #fff;
  border-bottom: 1px solid #ebeef5;

  .page-title {
    margin-left: 15px;
    font-size: 16px;
    font-weight: 500;
    color: #303133;
  }
}

.page-main {
  flex: 1;
  padding: 10px;
  overflow: hidden;
}
</style>
