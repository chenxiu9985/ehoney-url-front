<template>
  <PageContent v-loading="loading">
    <div slot="form" class="page-form">
      <form-item label="查询内容">
        <el-input placeholder="请输入查询内容" v-model="searchParams.Payload" clearable @keyup.enter.native="search"></el-input>
      </form-item>
      <div class="btn-box">
        <el-button size="small" type="primary" @click="search">查询</el-button>
      </div>
    </div>
    <div class="page-main">
      <ListLayout>
        <el-table slot="list" :data="tableData" width="100%" height="100%" :border="false">
          <el-table-column label="诱导目标" prop="TargetName"></el-table-column>
          <el-table-column label="蜜链数量" prop="ChainCount"></el-table-column>
          <el-table-column label="命中次数" prop="HitCount"></el-table-column>
          <el-table-column label="独立访问IP数" prop="UniqueIPCount"></el-table-column>
          <el-table-column label="被访问率" prop="VisitRate">
            <template v-slot="{row}">
              <span>{{ formatPercent(row.VisitRate) }}</span>
            </template>
          </el-table-column>
          <el-table-column label="识破率" prop="DetectRate">
            <template v-slot="{row}">
              <span>{{ formatPercent(row.DetectRate) }}</span>
            </template>
          </el-table-column>
          <el-table-column label="平均停留时长" prop="AvgStayTime"></el-table-column>
          <el-table-column label="操作" width="100px">
            <template v-slot="{row}">
              <el-button type="primary" size="mini" icon="el-icon-view" circle @click="goToDetail(row)"></el-button>
            </template>
          </el-table-column>
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
              TargetName: '电商蜜链站点',
              ChainCount: 10,
              HitCount: 156,
              UniqueIPCount: 45,
              VisitRate: 0.785,
              DetectRate: 0.052,
              AvgStayTime: '2分30秒'
            },
            {
              ID: '2',
              TargetName: '金融蜜链站点',
              ChainCount: 15,
              HitCount: 89,
              UniqueIPCount: 32,
              VisitRate: 0.623,
              DetectRate: 0.089,
              AvgStayTime: '3分15秒'
            },
            {
              ID: '3',
              TargetName: '政务蜜链站点',
              ChainCount: 8,
              HitCount: 234,
              UniqueIPCount: 78,
              VisitRate: 0.912,
              DetectRate: 0.034,
              AvgStayTime: '1分45秒'
            },
            {
              ID: '4',
              TargetName: '医疗蜜链站点',
              ChainCount: 12,
              HitCount: 67,
              UniqueIPCount: 28,
              VisitRate: 0.456,
              DetectRate: 0.123,
              AvgStayTime: '4分20秒'
            },
            {
              ID: '5',
              TargetName: '教育蜜链站点',
              ChainCount: 6,
              HitCount: 45,
              UniqueIPCount: 19,
              VisitRate: 0.389,
              DetectRate: 0.067,
              AvgStayTime: '2分50秒'
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
     * 格式化百分比
     */
    formatPercent (value) {
      if (value === undefined || value === null) return '-';
      return (value * 100).toFixed(1) + '%';
    },

    /**
     * 跳转到详情页
     */
    goToDetail (row) {
      this.$router.push({
        path: '/honey-chain/chain-detail',
        query: {
          targetId: row.ID,
          targetName: row.TargetName
        }
      });
    }
  }
}
</script>
