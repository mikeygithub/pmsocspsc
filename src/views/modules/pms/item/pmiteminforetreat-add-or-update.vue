<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <!--<el-form-item label="项目立项申请Id" prop="itemInfoId">-->
      <!--<el-input v-model="dataForm.itemInfoId" placeholder="项目立项申请Id"></el-input>-->
    <!--</el-form-item>-->
    <el-form-item label="回退意见" prop="retreatAdvise">
      <!--<el-input v-model="dataForm.retreatAdvise" placeholder="回退意见"></el-input>-->
      <el-input
        type="textarea"
        :rows="5"
        placeholder="请输入内容"
        v-model="dataForm.retreatAdvise">
      </el-input>
    </el-form-item>
    <!--<el-form-item label="删除标识" prop="retreatIsDel">-->
      <!--<el-input v-model="dataForm.retreatIsDel" placeholder="删除标识"></el-input>-->
    <!--</el-form-item>-->
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
          retreatId: 0,
          itemInfoId: '',
          retreatAdvise: '',
          retreatIsDel: ''
        },
        dataRule: {
          itemInfoId: [
            { required: true, message: '项目立项申请Id不能为空', trigger: 'blur' }
          ],
          retreatAdvise: [
            { required: true, message: '回退意见不能为空', trigger: 'blur' }
          ],
          retreatIsDel: [
            { required: true, message: '删除标识不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.itemInfoId = id
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.retreatId) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmiteminforetreat/info/${this.dataForm.retreatId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.itemInfoId = data.pmItemInfoRetreat.itemInfoId
                this.dataForm.retreatAdvise = data.pmItemInfoRetreat.retreatAdvise
                this.dataForm.retreatIsDel = data.pmItemInfoRetreat.retreatIsDel
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
              url: this.$http.adornUrl(`/pms/pmiteminforetreat/${!this.dataForm.retreatId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'retreatId': this.dataForm.retreatId || undefined,
                'itemInfoId': this.dataForm.itemInfoId,
                'retreatAdvise': this.dataForm.retreatAdvise,
                'retreatIsDel': 0
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                // 更新状态
                this.$http({
                  url: this.$http.adornUrl('/pms/pmiteminfo/apply'),
                  method: 'post',
                  params: this.$http.adornParams({
                    'id': this.dataForm.itemInfoId,
                    'status': 3
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
