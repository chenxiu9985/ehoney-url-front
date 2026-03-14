<template>
  <el-dialog title="命中分析详情" visible width="600px" :close-on-click-modal="false" :before-close="close" append-to-body>
    <el-descriptions :column="2" border>
      <el-descriptions-item label="诱导目标">{{ detailData.TargetName }}</el-descriptions-item>
      <el-descriptions-item label="蜜链数量">{{ detailData.ChainCount }}</el-descriptions-item>
      <el-descriptions-item label="命中次数">{{ detailData.HitCount }}</el-descriptions-item>
      <el-descriptions-item label="独立访问IP数">{{ detailData.UniqueIPCount }}</el-descriptions-item>
      <el-descriptions-item label="被访问率">{{ formatPercent(detailData.VisitRate) }}</el-descriptions-item>
      <el-descriptions-item label="识破率">{{ formatPercent(detailData.DetectRate) }}</el-descriptions-item>
      <el-descriptions-item label="平均停留时长" :span="2">{{ detailData.AvgStayTime }}</el-descriptions-item>
    </el-descriptions>
    <span slot="footer" class="dialog-footer">
      <el-button @click="close">关 闭</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  props: {
    detailData: {
      type: Object,
      default: () => ({})
    }
  },

  methods: {
    /**
     * 格式化百分比
     */
    formatPercent (value) {
      if (value === undefined || value === null) return '-';
      return (value * 100).toFixed(1) + '%';
    },

    /**
     * 关闭弹窗
     */
    close () {
      this.$emit('close');
    }
  }
}
</script>
