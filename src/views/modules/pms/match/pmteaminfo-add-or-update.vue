<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    width="60%"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-row>
        <el-col :span="12">
          <el-form-item label="赛事" prop="itemInfoId">
              <el-select v-model="dataForm.itemInfoId" filterable placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="报名时间" prop="signUpTime">
            <div class="block">
              <el-date-picker
                v-model="dataForm.signUpTime"
                align="right"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="报名时间">
              </el-date-picker>
            </div>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="团队编号" prop="teamCode">
            <el-input v-model="dataForm.teamCode" placeholder="团队编号"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="项目编号" prop="itemInfoCode">
            <el-input v-model="dataForm.itemInfoCode" placeholder="项目编号"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
    <el-row>
      <el-col :span="24">
        <el-form-item label="赛题" prop="matchTitle">
          <el-input v-model="dataForm.matchTitle" placeholder="赛题"></el-input>
        </el-form-item>
      </el-col>
    </el-row>
      <!--成员信息列表 s-->
      <el-form-item label="成员列表">
      <template>
        <el-table
          :data="teamMemberList"
          style="width: 100%">
          <el-table-column
            label="学号"
            width="180">
            <template slot-scope="scope">
              <!--<i class="el-icon-time"></i>-->
              <span style="margin-left: 10px">{{ scope.row.personCode }}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="姓名"
            width="180">
            <template slot-scope="scope">
              <el-popover trigger="hover" placement="top">
                <p>学号: {{ scope.row.personCode }}</p>
                <p>姓名: {{ scope.row.personName }}</p>
                <p>学院: {{ scope.row.personInstitute }}</p>
                <p>班级: {{ scope.row.personClass }}</p>
                <p>年级: {{ scope.row.personGrade }}</p>
                <p>专业: {{ scope.row.personMajor }}</p>
                <p>邮箱: {{ scope.row.personEmail }}</p>
                <p>电话: {{ scope.row.personPhone }}</p>
                <div slot="reference" class="name-wrapper">
                  <el-tag size="medium">{{ scope.row.personName }}</el-tag>
                </div>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column
            label="学院"
            width="180">
            <template slot-scope="scope">
              <el-popover trigger="hover" placement="top">
                <p>学号: {{ scope.row.personCode }}</p>
                <p>姓名: {{ scope.row.personName }}</p>
                <p>学院: {{ scope.row.personInstitute }}</p>
                <p>班级: {{ scope.row.personClass }}</p>
                <p>年级: {{ scope.row.personGrade }}</p>
                <p>专业: {{ scope.row.personMajor }}</p>
                <p>邮箱: {{ scope.row.personEmail }}</p>
                <p>电话: {{ scope.row.personPhone }}</p>
                <div slot="reference" class="name-wrapper">
                  <el-tag size="medium">{{ scope.row.personInstitute }}</el-tag>
                </div>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column
            label="班级"
            width="180">
            <template slot-scope="scope">
              <el-popover trigger="hover" placement="top">
                <p>学号: {{ scope.row.personCode }}</p>
                <p>姓名: {{ scope.row.personName }}</p>
                <p>学院: {{ scope.row.personInstitute }}</p>
                <p>班级: {{ scope.row.personClass }}</p>
                <p>年级: {{ scope.row.personGrade }}</p>
                <p>专业: {{ scope.row.personMajor }}</p>
                <p>邮箱: {{ scope.row.personEmail }}</p>
                <p>电话: {{ scope.row.personPhone }}</p>
                <div slot="reference" class="name-wrapper">
                  <el-tag size="medium">{{ scope.row.personClass }}</el-tag>
                </div>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column label="操作" width="150">
            <template slot-scope="scope" >
              <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
              <!--<el-popover-->
                <!--placement="top"-->
                <!--width="160"-->
                <!--v-model="delVisible">-->
                <!--<p>确定删除该成员吗？</p>-->
                <!--<div style="text-align: right; margin: 0">-->
                  <!--<el-button size="mini" type="text" @click="delVisible = false">取消</el-button>-->
                  <!--<el-button type="danger" size="mini" @click="handleDelete(scope.$index, scope.row)">确定</el-button>-->
                <!--</div>-->
                <!--<el-button slot="reference" size="mini">删除</el-button>-->
              <!--</el-popover>-->
            </template>
          </el-table-column>
        </el-table>
      </template>
      </el-form-item>
      <!--成员信息列表 e-->
      <el-row>
        <el-col>
          <el-form-item label="操作">
            <el-button @click="addMemberHandle()">添加成员</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
    <Member v-if="addMemberVisible" ref="addMember" @refreshDataList="refreshMember"></Member>
  </el-dialog>
</template>

<script>
  import Member from './pmteampersoninfo-add-or-update'

  export default {
    data () {
      return {
        visible: false,
        addMemberVisible: false,
        delVisible: false,
        teamMemberList: [],
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
        dataRule: {
          teamCode: [
            { required: true, message: '团队编号不能为空', trigger: 'blur' }
          ],
          itemInfoId: [
            { required: true, message: '立项id不能为空', trigger: 'blur' }
          ],
          itemInfoCode: [
            { required: true, message: '项目编号不能为空', trigger: 'blur' }
          ],
          matchTitle: [
            { required: true, message: '赛题不能为空', trigger: 'blur' }
          ],
          signUpTime: [
            { required: true, message: '报名时间不能为空', trigger: 'blur' }
          ],
          awardGrade: [
            { required: true, message: '获奖级别：国家级：区级不能为空', trigger: 'blur' }
          ],
          awardInfo: [
            { required: true, message: '获奖不能为空', trigger: 'blur' }
          ],
          teamInfoIsDel: [
            { required: true, message: '删除标识不能为空', trigger: 'blur' }
          ]
        },
        options: [],
        value: ''
      }
    },
    components: {
      Member
    },
    methods: {
      init (id) {
        this.dataForm.teamId = id || 0
        this.teamMemberList = []
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          // 请求赛事数据
          this.$http({
            url: this.$http.adornUrl('/pms/pmiteminfo/list'),
            method: 'get',
            params: this.$http.adornParams({
              'page': 1,
              'limit': 10000,
              'itemInfoStatus': 2,
              'userId': this.$store.state.user.id
            })
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.options = []
              data.page.list.forEach(item => {
                this.options.push({value: item.itemInfoId + '', label: item.matchName})
                if (this.dataForm.teamId) {
                  this.$http({
                    url: this.$http.adornUrl(`/pms/pmteaminfo/info/${this.dataForm.teamId}`),
                    method: 'get',
                    params: this.$http.adornParams()
                  }).then(({data}) => {
                    if (data && data.code === 0) {
                      this.dataForm.teamCode = data.pmTeamInfo.teamCode
                      this.dataForm.itemInfoId = data.pmTeamInfo.itemInfoId
                      this.dataForm.itemInfoCode = data.pmTeamInfo.itemInfoCode
                      this.dataForm.matchTitle = data.pmTeamInfo.matchTitle
                      this.dataForm.signUpTime = data.pmTeamInfo.signUpTime
                      this.dataForm.awardGrade = data.pmTeamInfo.awardGrade
                      this.dataForm.awardInfo = data.pmTeamInfo.awardInfo
                      this.teamMemberList = data.pmTeamInfo.pmTeamPersonInfoEntities
                    }
                  })
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/pms/pmteaminfo/${!this.dataForm.teamId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'teamId': this.dataForm.teamId || undefined,
                'teamCode': this.dataForm.teamCode,
                'itemInfoId': this.dataForm.itemInfoId,
                'itemInfoCode': this.dataForm.itemInfoCode,
                'matchTitle': this.dataForm.matchTitle,
                'signUpTime': this.dataForm.signUpTime,
                'awardGrade': this.dataForm.awardGrade,
                'awardInfo': this.dataForm.awardInfo,
                'userId': this.$store.state.user.id,
                'pmTeamPersonInfoEntities': this.teamMemberList
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
      },
      // 新增成员
      addMemberHandle () {
        this.addMemberVisible = true
        this.$nextTick(() => {
          this.$refs['addMember'].init()
        })
      },
      refreshMember (data, index) {
        this.addMemberVisible = false
        if (index !== null) { // 更新标识
          this.teamMemberList.splice(index, 1)
        }
        this.teamMemberList.push(data)
      },
      // 编辑
      handleEdit (index, row) {
        this.addMemberVisible = true
        this.$nextTick(() => {
          this.$refs['addMember'].init(row, index)
        })
      },
      // 删除
      handleDelete (index, row) {
        this.teamMemberList.splice(index, 1)
      }
    }
  }
</script>
