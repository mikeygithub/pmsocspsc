<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.key" placeholder="参数名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('pms:pmiteminfo:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('pms:pmiteminfo:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item>
    </el-form>
    <el-card>
      <el-radio-group v-model="status" @change="getDataList">
        <el-radio label="1">未审批</el-radio>
        <el-radio label="2">已审批</el-radio>
      </el-radio-group>
    </el-card>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        prop="itemInfoId"
        header-align="center"
        align="center"
        label="序号">
      <template slot-scope="props">
        <p v-text="props.$index+1"></p>
      </template>
      </el-table-column>
      <el-table-column
        prop="matchName"
        header-align="center"
        align="center"
        label="赛事名称">
      </el-table-column>
      <el-table-column
        prop="matchUnit"
        header-align="center"
        align="center"
        label="组赛单位">
      </el-table-column>
      <el-table-column
        prop="matchType"
        header-align="center"
        align="center"
        label="赛制">
        <template slot-scope="scope">
          <el-tag type="small" :key="id" v-for="(type,id) in matchTypeList" v-if="scope.row.matchType === type.value" v-text="type.label"></el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="userId"
        header-align="center"
        align="center"
        label="项目负责人">
      </el-table-column>
      <el-table-column
        prop="matchStartTime"
        header-align="center"
        align="center"
        label="竞赛起始日期">
      </el-table-column>
      <el-table-column
        prop="matchEndTime"
        header-align="center"
        align="center"
        label="竞赛结束日期">
      </el-table-column>
      <el-table-column
        prop="matchMajor"
        header-align="center"
        align="center"
        label="专业">
      </el-table-column>
      <el-table-column
        prop="hostUnit"
        header-align="center"
        align="center"
        label="竞赛主办单位">
      </el-table-column>
      <el-table-column
        prop="undertakeUnit"
        header-align="center"
        align="center"
        label="竞赛承办单位">
      </el-table-column>
      <el-table-column
        prop="itemInfoTime"
        header-align="center"
        align="center"
        label="申请立项日期">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="matchSign"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="论证组赛的目的和意义">-->
      <!--</el-table-column>-->
      <el-table-column
        prop="itemInfoStatus"
        header-align="center"
        align="center"
        label="审核状态">
        <template slot-scope="scope">
          <el-tag type="small" v-if="scope.row.itemInfoStatus === 0"><span>未提交</span></el-tag>
          <el-tag type="small" v-if="scope.row.itemInfoStatus === 1"><span>待审核</span></el-tag>
          <el-tag type="small" v-if="scope.row.itemInfoStatus === 2"><span>通过</span></el-tag>
          <el-tag type="small" v-if="scope.row.itemInfoStatus === 3"><span>未通过</span></el-tag>
        </template>
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="itemInfoFinish"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="是否已经结题">-->
      <!--</el-table-column>-->
      <!--<el-table-column-->
        <!--prop="itemInfoIsDel"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="删除标识">-->
      <!--</el-table-column>-->
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small"  @click="detailHandle(scope.row.itemInfoId)">详情</el-button>
          <template v-if="scope.row.itemInfoStatus === 1">
            <el-button type="text" size="small" @click="submitHandle(scope.row.itemInfoId,2)">通过</el-button>
            <el-button type="text" size="small" @click="retreatAddHandle(scope.row.itemInfoId)">不通过</el-button>
          </template>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    <RetreatAdd v-if="retreatAddVisible" ref="retreatAdd" @refreshDataList="getDataList"></RetreatAdd>
    <Retreat v-if="retreatVisible" ref="retreat" @refreshDataList="getDataList"></Retreat>
    <Detail v-if="detailVisible" ref="detail" @refreshDataList="getDataList"></Detail>
  </div>
</template>

<script>
  import AddOrUpdate from './pmiteminfo-add-or-update'
  import RetreatAdd from './pmiteminforetreat-add-or-update'
  import Retreat from './pmiteminforetreat-list'
  import Detail from './item-info'

  export default {
    data () {
      return {
        dataForm: {
          key: ''
        },
        matchTypeList: [{
          value: 0,
          label: '团体赛'
        }, {
          value: 1,
          label: '个人赛'
        }],
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        status: 1,
        dataListLoading: false,
        dataListSelections: [],
        retreatVisible: false,
        addOrUpdateVisible: false,
        retreatAddVisible: false,
        detailVisible: false
      }
    },
    components: {
      AddOrUpdate,
      RetreatAdd,
      Retreat,
      Detail
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/pms/pmiteminfo/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'key': this.dataForm.key,
            'itemInfoStatus': this.status
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      // 回退
      retreatAddHandle (id) {
        this.retreatAddVisible = true
        this.$nextTick(() => {
          this.$refs.retreatAdd.init(id)
        })
      },
      // 不通过意见
      retreatHandle (id) {
        this.retreatVisible = true
        this.$nextTick(() => {
          this.$refs.retreat.init(id)
        })
      },
      // 详情
      detailHandle (id) {
        this.detailVisible = true
        this.$nextTick(() => {
          this.$refs.detail.init(id)
        })
      },
      // 审批
      submitHandle (id, status) {
        this.$confirm(`确定提交操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/pms/pmiteminfo/apply'),
            method: 'post',
            params: this.$http.adornParams({
              'id': id,
              'status': status
            })
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.itemInfoId
        })
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/pms/pmiteminfo/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      }
    }
  }
</script>
