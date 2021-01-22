<template>
  <div>
    <h1>ADMIN</h1>
    <div style="display: flex">
      <div style="width: 40vw">
        <div>设置</div>
        <el-form size="small" label-width="150px">
          <el-form-item label="本轮抽取">
            <el-input-number v-model="count"></el-input-number>位
          </el-form-item>
          <el-form-item label="允许重复中奖">
            <el-switch v-model="repeat"></el-switch>
          </el-form-item>
          <el-form-item label="页面背景图">
            <el-input v-model="backgroundImage"></el-input>
          </el-form-item>
          <el-form-item label="抽奖按钮文本1">
            <el-input v-model="buttonText"></el-input>
          </el-form-item>
          <el-form-item label="抽奖按钮文本2">
            <el-input v-model="buttonText2"></el-input>
          </el-form-item>
          <el-form-item label="抽奖按钮图片">
            <el-input v-model="buttonImage" placeholder="请填写图片URL"></el-input>
          </el-form-item>
          <el-form-item label="抽奖按钮字体大小">
            <el-input-number v-model="buttonFontSize"></el-input-number>
          </el-form-item>
          <el-form-item label="">
            <el-button @click="save">保存</el-button>
          </el-form-item>
        </el-form>
      </div>
      <div style="flex: 1">
        <div>中奖信息</div>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  data() {
    return {
      count: 0,
      repeat: false,
      backgroundImage: '',
      buttonText: '开始',
      buttonText2: '结束',
      buttonImage: '',
      buttonFontSize: 16,
    };
  },
  methods: {
    save() {
      console.log(JSON.stringify(this.$data))
      localStorage.setItem('config', JSON.stringify(this.$data));
    }
  },
  mounted() {
    const config = JSON.parse(localStorage.getItem('config') || '{}');
    Object.assign(this.$data, config);
  }
};
</script>
