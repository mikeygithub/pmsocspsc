<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible" width="30%"
    append-to-body>
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <!--<el-row>-->
        <!--<el-col :span="12">-->
          <el-form-item label="所获奖项" prop="awardInfo">
            <!--<el-input v-model="dataForm.awardInfo" placeholder="所获奖项"></el-input>-->
              <el-select v-model="dataForm.awardInfo" filterable placeholder="请选择">
                <el-option
                  v-for="item in awardList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
          </el-form-item>
        <!--</el-col>-->
        <!--<el-col :span="12">-->
          <el-form-item label="获奖等级" prop="awardGrade">
            <!--<el-input v-model="dataForm.awardGrade" placeholder="获奖等级"></el-input>-->
            <el-select v-model="dataForm.awardGrade" filterable placeholder="请选择">
              <el-option
                v-for="item in awardGradeList"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        <!--</el-col>-->
      <!--</el-row>-->
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
        index: null,
        dataForm: {
          teamId: 0,
          teamCode: '',
          itemInfoId: '',
          itemInfoCode: '',
          matchTitle: '',
          signUpTime: '',
          awardGrade: '',
          awardInfo: '',
          teamInfoIsDel: 0
        },
        awardGradeList: [
          {label: '国家级', value: '1'},
          {label: '区级', value: '2'},
          {label: '校级', value: '3'}
        ],
        awardList: [
          {label: '特等奖', value: '1'},
          {label: '一等奖', value: '2'},
          {label: '二等奖', value: '3'},
          {label: '三等奖', value: '4'},
          {label: '优秀奖', value: '5'},
          {label: '参与奖', value: '6'},
          {label: '无', value: '7'}
        ],
        dataRule: {
          awardInfo: [
            {required: true, message: '获奖信息不能为空', trigger: 'blur'}
          ],
          awardGrade: [
            {required: true, message: '获奖等级不能为空', trigger: 'blur'}
          ]
        }
      }
    },
    methods: {
      init (item, index) {
        this.visible = true
        this.$nextTick(() => {
          // this.$refs['dataForm'].resetFields()
          // if (item) {
          this.index = index
          this.dataForm = JSON.parse(JSON.stringify(item))
          // }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.visible = false
            this.$emit('refreshDataList', this.dataForm, this.index)
          }
        })
      }
    }
  }
</script>
