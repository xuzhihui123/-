<template>
  <div class="app-container">
    <el-form ref="form" :model="formData" label-width="100px" :rules="dataRules">
      <el-form-item label="医院名称：" prop="hospName">
        <el-input v-model="formData.hospName"></el-input>
      </el-form-item>
      <el-form-item label="医院地址：" prop="hospAddr">
        <el-input v-model="formData.hospAddr"></el-input>
      </el-form-item>
      <el-form-item label="备注：">
        <el-input v-model="formData.remark"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitAdd" :loading="loading">确认</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  //导入network
  import {addHosp} from "@/api/hospitalManage";

  export default {
    name: "index",
    data() {
      return {
        formData: {
          hospName: '',
          hospAddr: '',
          remark: ''
        },
        loading: false,
        dataRules: {
          hospName: [
            {required: true, message: '请输入医院名称', trigger: 'blur'}
          ],
          hospAddr: [
            {required: true, message: '请输入医院地址', trigger: 'blur'}
          ]
        }
      }
    },
    methods: {
      submitAdd() {
        this.$refs.form.validate(async r => {
          if (r) {
            let {hospName, hospAddr, remark} = this.formData
            this.loading = true
            let d = await addHosp({hospName, hospAddr, remark})
            if (d.code === 200) {
              this.$message({
                type: 'success',
                message: '添加成功！'
              })
              this.loading = false
              for (let key in this.formData) {
                this.formData[key] = ""
              }
            } else {
              this.loading = false
              return this.$message({
                type: 'error',
                message: '添加失败！'
              })
            }
          } else {
            return this.$message({
              type: 'warning',
              message: '请检查表单信息是否正确！'
            })
          }
        })
      }
    }
  }
</script>

<style scoped lang="less">

</style>
