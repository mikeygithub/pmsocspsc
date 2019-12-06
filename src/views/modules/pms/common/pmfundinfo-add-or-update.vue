<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="参赛注册费" prop="registerCost">
      <el-input v-model="dataForm.registerCost" placeholder="参赛注册费"></el-input>
    </el-form-item>
    <el-form-item label="差旅费" prop="travelCost">
      <el-input v-model="dataForm.travelCost" placeholder="差旅费"></el-input>
    </el-form-item>
    <el-form-item label="培训费" prop="trainCost">
      <el-input v-model="dataForm.trainCost" placeholder="培训费"></el-input>
    </el-form-item>
    <el-form-item label="评审费" prop="reviewCost">
      <el-input v-model="dataForm.reviewCost" placeholder="评审费"></el-input>
    </el-form-item>
    <el-form-item label="指导费" prop="guideCost">
      <el-input v-model="dataForm.guideCost" placeholder="指导费"></el-input>
    </el-form-item>
    <el-form-item label="领队费" prop="leaderCost">
      <el-input v-model="dataForm.leaderCost" placeholder="领队费"></el-input>
    </el-form-item>
    <el-form-item label="组织费" prop="organizeCost">
      <el-input v-model="dataForm.organizeCost" placeholder="组织费"></el-input>
    </el-form-item>
    <el-form-item label="耗材费" prop="consumablesCost">
      <el-input v-model="dataForm.consumablesCost" placeholder="耗材费"></el-input>
    </el-form-item>
    <el-form-item label="奖金" prop="awardCost">
      <el-input v-model="dataForm.awardCost" placeholder="奖金"></el-input>
    </el-form-item>
    <el-form-item label="其他费用" prop="anotherCost">
      <el-input v-model="dataForm.anotherCost" placeholder="其他费用"></el-input>
    </el-form-item>
    <el-form-item label="合计" prop="totalCost">
      <el-input v-model="dataForm.totalCost" placeholder="合计"></el-input>
    </el-form-item>
    <el-form-item label="删除标识" prop="budgetInfoIsDel">
      <el-input v-model="dataForm.budgetInfoIsDel" placeholder="删除标识"></el-input>
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
          budgetId: 0,
          registerCost: '',
          travelCost: '',
          trainCost: '',
          reviewCost: '',
          guideCost: '',
          leaderCost: '',
          organizeCost: '',
          consumablesCost: '',
          awardCost: '',
          anotherCost: '',
          totalCost: '',
          budgetInfoIsDel: ''
        },
        dataRule: {
          registerCost: [
            { required: true, message: '参赛注册费不能为空', trigger: 'blur' }
          ],
          travelCost: [
            { required: true, message: '差旅费不能为空', trigger: 'blur' }
          ],
          trainCost: [
            { required: true, message: '培训费不能为空', trigger: 'blur' }
          ],
          reviewCost: [
            { required: true, message: '评审费不能为空', trigger: 'blur' }
          ],
          guideCost: [
            { required: true, message: '指导费不能为空', trigger: 'blur' }
          ],
          leaderCost: [
            { required: true, message: '领队费不能为空', trigger: 'blur' }
          ],
          organizeCost: [
            { required: true, message: '组织费不能为空', trigger: 'blur' }
          ],
          consumablesCost: [
            { required: true, message: '耗材费不能为空', trigger: 'blur' }
          ],
          awardCost: [
            { required: true, message: '奖金不能为空', trigger: 'blur' }
          ],
          anotherCost: [
            { required: true, message: '其他费用不能为空', trigger: 'blur' }
          ],
          totalCost: [
            { required: true, message: '合计不能为空', trigger: 'blur' }
          ],
          budgetInfoIsDel: [
            { required: true, message: '删除标识不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.budgetId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.budgetId) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmfundinfo/info/${this.dataForm.budgetId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.registerCost = data.pmFundInfo.registerCost
                this.dataForm.travelCost = data.pmFundInfo.travelCost
                this.dataForm.trainCost = data.pmFundInfo.trainCost
                this.dataForm.reviewCost = data.pmFundInfo.reviewCost
                this.dataForm.guideCost = data.pmFundInfo.guideCost
                this.dataForm.leaderCost = data.pmFundInfo.leaderCost
                this.dataForm.organizeCost = data.pmFundInfo.organizeCost
                this.dataForm.consumablesCost = data.pmFundInfo.consumablesCost
                this.dataForm.awardCost = data.pmFundInfo.awardCost
                this.dataForm.anotherCost = data.pmFundInfo.anotherCost
                this.dataForm.totalCost = data.pmFundInfo.totalCost
                this.dataForm.budgetInfoIsDel = data.pmFundInfo.budgetInfoIsDel
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
              url: this.$http.adornUrl(`/pms/pmfundinfo/${!this.dataForm.budgetId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'budgetId': this.dataForm.budgetId || undefined,
                'registerCost': this.dataForm.registerCost,
                'travelCost': this.dataForm.travelCost,
                'trainCost': this.dataForm.trainCost,
                'reviewCost': this.dataForm.reviewCost,
                'guideCost': this.dataForm.guideCost,
                'leaderCost': this.dataForm.leaderCost,
                'organizeCost': this.dataForm.organizeCost,
                'consumablesCost': this.dataForm.consumablesCost,
                'awardCost': this.dataForm.awardCost,
                'anotherCost': this.dataForm.anotherCost,
                'totalCost': this.dataForm.totalCost,
                'budgetInfoIsDel': this.dataForm.budgetInfoIsDel
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
