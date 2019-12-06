<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form id="pdfDom" :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="120px">
      <el-row>
        <el-col :span="12">
          <el-form-item label="赛事名称" prop="matchName">
            <el-input v-model="dataForm.matchName" placeholder="赛事名称"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="组赛单位" prop="matchUnit">
            <el-input v-model="dataForm.matchUnit" placeholder="组赛单位"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="赛制" prop="matchType">
            <template>
              <el-select v-model="dataForm.matchType" placeholder="请选择赛制">
                <el-option
                  v-for="item in matchTypeList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </template>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="负责人" prop="userName">
            <el-input v-model="dataForm.userName" placeholder="项目负责人"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="专业" prop="matchMajor">
            <el-input v-model="dataForm.matchMajor" placeholder="专业"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="竞赛主办单位" prop="hostUnit">
            <el-input v-model="dataForm.hostUnit" placeholder="竞赛主办单位"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="竞赛承办单位" prop="undertakeUnit">
            <el-input v-model="dataForm.undertakeUnit" placeholder="竞赛承办单位"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="申请立项日期" prop="itemInfoTime">
            <!--<el-input v-model="dataForm.itemInfoTime" placeholder="申请立项日期"></el-input>-->
            <div class="block">
              <el-date-picker
                v-model="dataForm.itemInfoTime"
                align="right"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="申请立项日期">
              </el-date-picker>
            </div>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="竞赛起始日期" prop="matchStartTime">
            <div class="block">
              <el-date-picker
                v-model="dataForm.matchStartTime"
                align="right"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="竞赛起始日期">
              </el-date-picker>
            </div>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="竞赛结束日期" prop="matchEndTime">
            <div class="block">
              <el-date-picker
                v-model="dataForm.matchEndTime"
                align="right"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="竞赛结束日期">
              </el-date-picker>
            </div>
          </el-form-item>
        </el-col>
      </el-row>
      <br>
      <el-row>
        <el-col :span="12">
          <el-form-item label="参赛注册费" prop="pmFundInfoEntity.registerCost">
            <el-input v-model="dataForm.pmFundInfoEntity.registerCost" placeholder="参赛注册费" type="number" min=0 ></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="差旅费" prop="pmFundInfoEntity.travelCost">
            <el-input v-model="dataForm.pmFundInfoEntity.travelCost" placeholder="差旅费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="培训费" prop="pmFundInfoEntity.trainCost">
            <el-input v-model="dataForm.pmFundInfoEntity.trainCost" placeholder="培训费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="评审费" prop="pmFundInfoEntity.reviewCost">
            <el-input v-model="dataForm.pmFundInfoEntity.reviewCost" placeholder="评审费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="12">
          <el-form-item label="指导费" prop="pmFundInfoEntity.guideCost">
            <el-input v-model="dataForm.pmFundInfoEntity.guideCost" placeholder="指导费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="领队费" prop="pmFundInfoEntity.leaderCost">
            <el-input v-model="dataForm.pmFundInfoEntity.leaderCost" placeholder="领队费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="组织费" prop="pmFundInfoEntity.organizeCost">
            <el-input v-model="dataForm.pmFundInfoEntity.organizeCost" placeholder="组织费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="耗材费" prop="pmFundInfoEntity.consumablesCost">
            <el-input v-model="dataForm.pmFundInfoEntity.consumablesCost" placeholder="耗材费" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="奖金" prop="pmFundInfoEntity.awardCost">
            <el-input v-model="dataForm.pmFundInfoEntity.awardCost" placeholder="奖金" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="其他费用" prop="pmFundInfoEntity.anotherCost">
            <el-input v-model="dataForm.pmFundInfoEntity.anotherCost" placeholder="其他费用" type="number"  min=0></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item label="论证组赛的目的和意义" prop="matchSign">
        <el-input
          type="textarea"
          :rows="5"
          placeholder="请输入内容"
          v-model="dataForm.matchSign">
        </el-input>
      </el-form-item>
      <el-form-item label="附件">
        <el-upload
          class="upload-demo"
          :action="url"
          :on-remove="handleRemove"
          :on-success="uploadSuccess"
          multiple
          :limit="1"
          :on-exceed="handleExceed"
          :file-list="fileList">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">签字盖章后的项目立项申请书扫描为 PDF 文档</div>
        </el-upload>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">保存</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        htmlTitle: '立项申请书',
        fileList: [],
        url: '',
        dataForm: {
          itemInfoId: 0,
          matchName: '',
          matchUnit: '',
          matchType: '',
          userId: '',
          userName: '',
          matchStartTime: '',
          matchEndTime: '',
          matchMajor: '',
          hostUnit: '',
          undertakeUnit: '',
          itemInfoTime: '',
          matchSign: '',
          itemInfoStatus: 0,
          itemInfoFinish: 0,
          itemInfoIsDel: 0,
          pmFundInfoEntity: {
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
          pmItemAttachEntity: {
            attachId: '',
            itemInfoId: '',
            attachName: '',
            attachPath: '',
            attachIsDel: ''
          }
        },
        matchTypeList: [{
          value: 0,
          label: '团体赛'
        }, {
          value: 1,
          label: '个人赛'
        }],
        dataRule: {
          matchName: [
            {required: true, message: '赛事名称不能为空', trigger: 'blur'}
          ],
          matchUnit: [
            {required: true, message: '组赛单位不能为空', trigger: 'blur'}
          ],
          matchType: [
            {required: true, message: '赛制不能为空', trigger: 'blur'}
          ],
          userName: [
            {required: true, message: '项目负责人不能为空', trigger: 'blur'}
          ],
          matchStartTime: [
            {required: true, message: '竞赛起始日期不能为空', trigger: 'blur'}
          ],
          matchEndTime: [
            {required: true, message: '竞赛结束日期不能为空', trigger: 'blur'}
          ],
          matchMajor: [
            {required: true, message: '专业不能为空', trigger: 'blur'}
          ],
          hostUnit: [
            {required: true, message: '竞赛主办单位不能为空', trigger: 'blur'}
          ],
          undertakeUnit: [
            {required: true, message: '竞赛承办单位不能为空', trigger: 'blur'}
          ],
          itemInfoTime: [
            {required: true, message: '申请立项日期不能为空', trigger: 'blur'}
          ],
          matchSign: [
            {required: true, message: '论证组赛的目的和意义不能为空', trigger: 'blur'}
          ],
          pmFundInfoEntity: {
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
            ]
          }
        }
      }
    },
    methods: {
      init (id) {
        this.fileList = []
        this.dataForm.pmItemAttachEntity = null
        this.dataForm.itemInfoId = id || 0
        this.visible = true
        this.url = this.$http.adornUrl(`/pms/pmitemattach/upload?token=${this.$cookie.get('token')}`)
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.itemInfoId) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmiteminfo/info/${this.dataForm.itemInfoId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.matchName = data.pmItemInfo.matchName
                this.dataForm.matchUnit = data.pmItemInfo.matchUnit
                this.dataForm.matchType = data.pmItemInfo.matchType
                this.dataForm.userId = data.pmItemInfo.userId
                this.dataForm.userName = data.pmItemInfo.userName
                this.dataForm.matchStartTime = data.pmItemInfo.matchStartTime
                this.dataForm.matchEndTime = data.pmItemInfo.matchEndTime
                this.dataForm.matchMajor = data.pmItemInfo.matchMajor
                this.dataForm.hostUnit = data.pmItemInfo.hostUnit
                this.dataForm.undertakeUnit = data.pmItemInfo.undertakeUnit
                this.dataForm.itemInfoTime = data.pmItemInfo.itemInfoTime
                this.dataForm.matchSign = data.pmItemInfo.matchSign
                this.dataForm.itemInfoStatus = 0
                this.dataForm.itemInfoFinish = data.pmItemInfo.itemInfoFinish
                this.dataForm.itemInfoIsDel = data.pmItemInfo.itemInfoIsDel
                this.dataForm.pmFundInfoEntity = data.pmItemInfo.pmFundInfoEntity
                this.dataForm.pmItemAttachEntity = data.pmItemInfo.pmItemAttachEntity
                if (this.dataForm.pmItemAttachEntity.attachPath !== '') {
                  this.fileList.push({
                    url: this.dataForm.pmItemAttachEntity.attachPath,
                    name: this.dataForm.pmItemAttachEntity.attachName
                  })
                }
                console.log(data.pmItemInfo)
              }
            })
            console.log(this.dataForm.pmItemAttachEntity, 'color:blue')
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmiteminfo/${!this.dataForm.itemInfoId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'itemInfoId': this.dataForm.itemInfoId || undefined,
                'matchName': this.dataForm.matchName,
                'matchUnit': this.dataForm.matchUnit,
                'matchType': this.dataForm.matchType,
                'userId': this.$store.state.user.id,
                'userName': this.dataForm.userName,
                'matchStartTime': this.dataForm.matchStartTime,
                'matchEndTime': this.dataForm.matchEndTime,
                'matchMajor': this.dataForm.matchMajor,
                'hostUnit': this.dataForm.hostUnit,
                'undertakeUnit': this.dataForm.undertakeUnit,
                'itemInfoTime': this.dataForm.itemInfoTime,
                'matchSign': this.dataForm.matchSign,
                'itemInfoStatus': this.dataForm.itemInfoStatus,
                'itemInfoFinish': this.dataForm.itemInfoFinish,
                'itemInfoIsDel': this.dataForm.itemInfoIsDel,
                'pmFundInfoEntity': this.dataForm.pmFundInfoEntity,
                'pmItemAttachEntity': this.dataForm.pmItemAttachEntity
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.pmItemAttachEntity = null
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
      },
      // 上传成功
      uploadSuccess (res, file, fileList) {
        this.dataForm.pmItemAttachEntity = res.pmItemAttachEntity
      },
      // 移除文件
      handleRemove () {
        // if (this.dataForm.pmItemAttachEntity.attachId === null) {
        //   this.dataForm.pmItemAttachEntity = null
        // } else { // 修改
        this.dataForm.pmItemAttachEntity.attachIsDel = 1
        // }
      },
      // 文件超出个数
      handleExceed () {
      }
    }
  }
</script>
