<template>
  <el-dialog title="新建蜜链生成任务" visible width="560px" :close-on-click-modal="false" :before-close="close" append-to-body>
    <el-form ref="form" :model="form" :rules="rules" label-width="120px">
      <el-form-item label="场景类型" prop="SceneType">
        <el-select v-model="form.SceneType" placeholder="请选择场景类型" clearable style="width: 100%;">
          <el-option v-for="item in sceneTypeOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
      <el-form-item label="服务源URL" prop="SourceUrl">
        <el-input v-model="form.SourceUrl" placeholder="请输入服务源URL" clearable></el-input>
      </el-form-item>
      <el-form-item label="蜜链数量" prop="ChainCount">
        <el-input-number v-model="form.ChainCount" :min="1" :max="100" :precision="0" controls-position="right" style="width: 100%;"></el-input-number>
      </el-form-item>
      <el-form-item label="部署方式" prop="DeployMode">
        <el-select v-model="form.DeployMode" placeholder="请选择部署方式" clearable style="width: 100%;">
          <el-option v-for="item in deployModeOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
      <el-form-item label="蜜链生成方式" prop="GenerateMode">
        <el-select v-model="form.GenerateMode" placeholder="请选择生成方式" clearable style="width: 100%;">
          <el-option v-for="item in generateModeOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="close">取 消</el-button>
      <el-button type="primary" @click="validateForm" :loading="loading">保 存</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data () {
    return {
      loading: false,
      form: {
        SceneType: '',
        SourceUrl: '',
        ChainCount: 100,
        DeployMode: 'path',
        GenerateMode: 'gan'
      },
      rules: {
        SourceUrl: [
          { required: true, message: '请输入服务源URL', trigger: 'blur' },
          { type: 'url', message: '请输入正确的URL格式', trigger: 'blur' }
        ],
        ChainCount: [
          { required: true, message: '请输入蜜链数量', trigger: 'blur' },
          { type: 'integer', min: 1, message: '蜜链数量必须为正整数', trigger: 'blur' }
        ]
      },
      sceneTypeOptions: [
        { label: '社交平台', value: 'social' },
        { label: '办公系统', value: 'office' },
        { label: '内容系统', value: 'content' },
        { label: '邮件系统', value: 'email' }
      ],
      deployModeOptions: [
        { label: '路径发布', value: 'path' }
      ],
      generateModeOptions: [
        { label: 'GAN', value: 'gan' }
      ]
    }
  },

  methods: {
    /**
     * 校验表单
     */
    validateForm () {
      this.$refs.form.validate((valid) => {
        if (valid) {
          this.save();
        }
      });
    },

    /**
     * 保存
     */
    save () {
      this.loading = true;
      
      // Mock提交 - 打印表单数据
      console.log('新建蜜链生成任务表单数据：', this.form);
      
      // 模拟异步请求
      setTimeout(() => {
        this.loading = false;
        this.$message.success('新建成功！');
        this.$emit('save');
        this.close();
      }, 500);
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
