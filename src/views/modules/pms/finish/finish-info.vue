<template>
  <el-dialog
    :title="'详情'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <div id="info">
    <table class="el-table__empty-block" >
      <tr>
        <th colspan="12">立项信息</th>
      </tr>
      <tr>
        <th colspan="2">立项项目名称</th>
        <td colspan="2"><span v-text="dataForm.matchName"></span></td>

        <th colspan="2">组赛单位</th>
        <td colspan="2"><span v-text="dataForm.matchUnit"></span></td>

        <th colspan="2">赛制</th>
        <td colspan="2"><span v-for="(item,index) in matchTypeList" :key="index" v-if="dataForm.matchType === item.value" v-text="item.label"></span></td>
      </tr>
      <tr>
        <th colspan="2">负责人</th>
        <td colspan="2"><span v-text="dataForm.userName"></span></td>

        <th colspan="2">专业</th>
        <td colspan="2"><span v-text="dataForm.matchMajor"></span></td>

        <th colspan="2">竞赛主办单位</th>
        <td colspan="2"><span v-text="dataForm.hostUnit"></span></td>
      </tr>
      <tr>
        <th colspan="2">竞赛承办单位</th>
        <td colspan="2"><span v-text="dataForm.undertakeUnit"></span></td>

        <th colspan="2">申请立项日期</th>
        <td colspan="2"><span v-text="dataForm.itemInfoTime"></span></td>

        <th colspan="2">竞赛起始日期</th>
        <td colspan="2"><span v-text="dataForm.matchStartTime"></span></td>
      </tr>
      <tr>
        <th colspan="2">竞赛结束日期</th>
        <td colspan="5"><span v-text="dataForm.matchEndTime"></span></td>
        <th colspan="2">状态</th>
        <td colspan="3">
          <el-tag type="small" v-if="dataForm.itemInfoStatus === 0"><span>未提交</span></el-tag>
          <el-tag type="small" v-if="dataForm.itemInfoStatus === 1"><span>待审核</span></el-tag>
          <el-tag type="small" v-if="dataForm.itemInfoStatus === 2"><span>通过</span></el-tag>
          <el-tag type="small" v-if="dataForm.itemInfoStatus === 3"><span>未通过</span></el-tag>
        </td>
      </tr>
        <tr>
          <th>参赛注册费</th>
          <th>差旅费</th>
          <th>培训费</th>
          <th>评审费</th>
          <th>指导费</th>
          <th>领队费</th>
          <th>组织费</th>
          <th>耗材费</th>
          <th>奖金</th>
          <th>其他费用</th>
          <th colspan="2">合计</th>
      </tr>
      <tr>
        <td><span v-text="dataForm.pmFundInfoEntity.registerCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.travelCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.trainCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.reviewCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.guideCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.leaderCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.organizeCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.consumablesCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.awardCost"></span></td>
        <td><span v-text="dataForm.pmFundInfoEntity.anotherCost"></span></td>
        <td colspan="2"><span v-text="dataForm.pmFundInfoEntity.totalCost"></span></td>
      </tr>
      <tr style="height: auto">
        <th colspan="2">论证组赛的目的和意义</th>
        <td colspan="10" style="line-height: normal"><p><span v-text="dataForm.matchSign"></span></p></td>
      </tr>
      <tr>
        <th colspan="12">结题信息</th>
      </tr>
      <tr>
        <th>参赛注册费</th>
        <th>差旅费</th>
        <th>培训费</th>
        <th>评审费</th>
        <th>指导费</th>
        <th>领队费</th>
        <th>组织费</th>
        <th>耗材费</th>
        <th>奖金</th>
        <th>其他费用</th>
        <th colspan="2">合计</th>
      </tr>
      <tr>
        <td><span v-text="pmFundInfoEntity.registerCost"></span></td>
        <td><span v-text="pmFundInfoEntity.travelCost"></span></td>
        <td><span v-text="pmFundInfoEntity.trainCost"></span></td>
        <td><span v-text="pmFundInfoEntity.reviewCost"></span></td>
        <td><span v-text="pmFundInfoEntity.guideCost"></span></td>
        <td><span v-text="pmFundInfoEntity.leaderCost"></span></td>
        <td><span v-text="pmFundInfoEntity.organizeCost"></span></td>
        <td><span v-text="pmFundInfoEntity.consumablesCost"></span></td>
        <td><span v-text="pmFundInfoEntity.awardCost"></span></td>
        <td><span v-text="pmFundInfoEntity.anotherCost"></span></td>
        <td colspan="2"><span v-text="pmFundInfoEntity.totalCost"></span></td>
      </tr>
      <tr>
        <th colspan="2">队伍编号</th>
        <th colspan="2">队伍名称</th>
        <th colspan="2">所选赛题</th>
        <th colspan="2">报名时间</th>
        <th colspan="2">获奖信息</th>
        <th colspan="2">获奖等级</th>
      </tr>
      <tr v-for="(item,index) in teamList" :key="index">
        <td colspan="2"><span v-text="item.teamCode"></span></td>
        <td colspan="2"><span v-text="item.teamCode"></span></td>
        <td colspan="2"><span v-text="item.matchTitle"></span></td>
        <td colspan="2"><span v-text="item.signUpTime"></span></td>
        <td colspan="2"><span v-for="(info, index) in awardList" :key="index" v-if="info.value === item.awardInfo" v-text="info.label"></span></td>
        <td colspan="2"><span v-for="(info, index) in awardGradeList" :key="index" v-if="info.value === item.awardGrade" v-text="info.label"></span></td>
      </tr>
    </table>
    </div>
    <div id="info2" style="margin-top: 2rem">
      <table v-if="dataForm.pmItemAttachEntity!=null&&dataForm.pmItemAttachEntity.attachPath!=''" align="center"width="100%" border="0" cellpadding="0" cellspacing="0" style="table-layout: fixed;" >
        <tr align="center"><th colspan="3">立项附件信息</th></tr>
        <tr align="center">
          <th>附件</th>
          <td><span v-text="dataForm.pmItemAttachEntity.attachName"></span></td>
          <td><el-button @click="downloadAttach(dataForm.pmItemAttachEntity.attachName, dataForm.pmItemAttachEntity.attachPath)" :loading="downloadLoading">下载</el-button></td>
        </tr>
        <tr align="center" v-if="pmFinishAttachEntity !== null && pmFinishAttachEntity.attachPath!==''"><th colspan="3">结题附件信息</th></tr>
        <tr align="center" v-if="pmFinishAttachEntity !== null && pmFinishAttachEntity.attachPath!==''">
          <th>附件</th>
          <td><span v-text="pmFinishAttachEntity.attachName"></span></td>
          <td><el-button @click="downloadAttach(pmFinishAttachEntity.attachName, pmFinishAttachEntity.attachPath)" :loading="downloadLoading">下载</el-button></td>
        </tr>
      </table>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button v-print="'#info'">导出</el-button>
      <el-button type="primary" @click="visible = false">确认</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        htmlTitle: '立项申请书',
        downloadLoading: false,
        teamList: [],
        pmFundInfoEntity: '',
        pmFinishAttachEntity: {
          attachId: '',
          itemInfoId: '',
          attachName: '',
          attachPath: '',
          attachIsDel: ''
        },
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
            attachId: 0,
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
        ]
      }
    },
    methods: {
      init (finishInfoId, itemInfoId) {
        this.fileList = []
        this.dataForm.itemInfoId = itemInfoId || 0
        this.visible = true
        this.$nextTick(() => {
          if (this.dataForm.itemInfoId) {
            this.$http({ // 查询立项项目信息
              url: this.$http.adornUrl(`/pms/pmiteminfo/info/${this.dataForm.itemInfoId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                console.log(data.pmItemInfo)
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
                this.dataForm.itemInfoStatus = data.pmItemInfo.itemInfoStatus
                this.dataForm.itemInfoFinish = data.pmItemInfo.itemInfoFinish
                this.dataForm.itemInfoIsDel = data.pmItemInfo.itemInfoIsDel
                this.dataForm.pmFundInfoEntity = data.pmItemInfo.pmFundInfoEntity
                this.dataForm.pmItemAttachEntity = data.pmItemInfo.pmItemAttachEntity
              }
            })
          }
          // 结题信息
          this.$http({
            url: this.$http.adornUrl(`/pms/pmfinishinfo/info/` + finishInfoId),
            method: 'get',
            params: this.$http.adornParams()
          }).then(({data}) => {
            if (data && data.code === 0) {
              console.log(data.pmFinishInfo)
              this.pmFundInfoEntity = data.pmFinishInfo.pmFundInfoEntity
              this.teamList = data.pmFinishInfo.pmTeamInfoEntities
              this.pmFinishAttachEntity = data.pmFinishInfo.pmFinishAttachEntity
            }
          })
        })
      },
      // 下载附件
      downloadAttach (attachName, path) {
        this.downloadLoading = true
        this.$http({
          url: this.$http.adornUrl(`/pms/pmitemattach/download`),
          method: 'post',
          params: this.$http.adornParams({
            'filePath': path
          }),
          responseType: 'blob'
        }).then(response => {
          if (!response) {
            this.downloadLoading = false
            return
          }
          let url = window.URL.createObjectURL(new Blob([response.data]))
          let link = document.createElement('a')
          link.style.display = 'none'
          link.href = url
          link.setAttribute('download', attachName)
          document.body.appendChild(link)
          link.click()
          this.downloadLoading = false
        }).catch(err => {
          console.log(err)
          this.downloadLoading = false
        })
      }
    }
  }
</script>
<style>
  #info table{
    border-right: 1px solid #ebeef5;
    border-bottom: 1px solid #ebeef5;
  }
  #info th{
    border-left: 1px solid #ebeef5;
    border-top: 1px solid #ebeef5;
    white-space: nowrap;
    line-height: 3rem;
  }
  #info td{
    border-left: 1px solid #ebeef5;
    border-top: 1px solid #ebeef5;
    line-height: 3rem;
  }
  #info tr{
    border: 1px solid #ebeef5;
    padding: 10px 0;
  }
  #info p{
    text-indent:2em;
  }
  #info2 table{
    border-right: 1px solid #ebeef5;
    border-bottom: 1px solid #ebeef5;
  }
  #info2 th{
    border-left: 1px solid #ebeef5;
    border-top: 1px solid #ebeef5;
    white-space: nowrap;
    line-height: 3rem;
  }
  #info2 td{
    border-left: 1px solid #ebeef5;
    border-top: 1px solid #ebeef5;
    line-height: 3rem;
  }
  #info2 tr{
    border: 1px solid #ebeef5;
    padding: 10px 0;
  }
  #info2 p{
    text-indent:2em;
  }
</style>
