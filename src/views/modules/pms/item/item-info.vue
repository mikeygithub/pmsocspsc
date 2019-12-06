<template>
  <el-dialog
    :title="'详情'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <div id="info">
    <table class="el-table__empty-block" >
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
    </table>
    </div>
    <table v-if="dataForm.pmItemAttachEntity!=null&&dataForm.pmItemAttachEntity.attachIsDel!==1&&dataForm.pmItemAttachEntity.attachPath!=''" align="center"width="100%" border="0" cellpadding="0" cellspacing="0" style="table-layout: fixed;" >
      <tr align="center">
        <th>附件</th>
        <td><span v-text="dataForm.pmItemAttachEntity.attachName"></span></td>
        <td><el-button @click="downloadAttach" :loading="downloadLoading">下载</el-button></td>
      </tr>
    </table>
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
        }]
      }
    },
    methods: {
      init (id) {
        this.fileList.pop()
        this.dataForm.itemInfoId = id || 0
        this.visible = true
        this.$nextTick(() => {
          // this.$refs['dataForm'].resetFields()
          if (this.dataForm.itemInfoId) {
            this.$http({
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
        })
      },
      // 下载附件
      downloadAttach () {
        this.downloadLoading = true
        this.$http({
          url: this.$http.adornUrl(`/pms/pmitemattach/download`),
          method: 'post',
          params: this.$http.adornParams({
            'filePath': this.dataForm.pmItemAttachEntity.attachPath
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
          link.setAttribute('download', this.dataForm.pmItemAttachEntity.attachName)
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
</style>
