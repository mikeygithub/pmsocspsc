<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="结题id" prop="finishInfoId">
      <el-input v-model="dataForm.finishInfoId" placeholder="结题id"></el-input>
    </el-form-item>
    <el-form-item label="附件名称" prop="attachName">
      <el-input v-model="dataForm.attachName" placeholder="附件名称"></el-input>
    </el-form-item>
    <el-form-item label="附件路径" prop="attachPath">
      <el-input v-model="dataForm.attachPath" placeholder="附件路径"></el-input>
    </el-form-item>
    <el-form-item label="删除标识" prop="attachIsDel">
      <el-input v-model="dataForm.attachIsDel" placeholder="删除标识"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          attachId: 0,
          finishInfoId: '',
          attachName: '',
          attachPath: '',
          attachIsDel: ''
        },
        dataRule: {
          finishInfoId: [
            { required: true, message: '结题id不能为空', trigger: 'blur' }
          ],
          attachName: [
            { required: true, message: '附件名称不能为空', trigger: 'blur' }
          ],
          attachPath: [
            { required: true, message: '附件路径不能为空', trigger: 'blur' }
          ],
          attachIsDel: [
            { required: true, message: '删除标识不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.attachId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.attachId) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmfinishattach/info/${this.dataForm.attachId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.finishInfoId = data.pmFinishAttach.finishInfoId
                this.dataForm.attachName = data.pmFinishAttach.attachName
                this.dataForm.attachPath = data.pmFinishAttach.attachPath
                this.dataForm.attachIsDel = data.pmFinishAttach.attachIsDel
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmfinishattach/${!this.dataForm.attachId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'attachId': this.dataForm.attachId || undefined,
                'finishInfoId': this.dataForm.finishInfoId,
                'attachName': this.dataForm.attachName,
                'attachPath': this.dataForm.attachPath,
                'attachIsDel': this.dataForm.attachIsDel
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
