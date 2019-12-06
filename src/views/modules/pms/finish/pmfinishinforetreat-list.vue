<template>
  <el-dialog
    :title="修改意见"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-table
        :data="dataList"
        border
        v-loading="dataListLoading"
        @selection-change="selectionChangeHandle"
        style="width: 100%;">
        <el-table-column
          prop="itemInfoId"
          header-align="center"
          align="center"
          label="序号"
        :width="100">
          <template slot-scope="props">
            <p v-text="props.$index+1"></p>
          </template>
        </el-table-column>
        <el-table-column
          prop="retreatAdvise"
          header-align="center"
          align="center"
          label="修改意见">
        </el-table-column>
      </el-table>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="visible = false">确定</el-button>
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
          finishInfoId: '',
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
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/pms/pmfinishinforetreat/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': 1,
            'limit': 1000,
            'finishInfoId': id
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
          this.visible = true
        })
      }
    }
  }
</script>
