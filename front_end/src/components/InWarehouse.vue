<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <el-card class="box-card" style="width: 900px;">
          <div slot="header" class="clearfix">
            <span style="font-size: 16px;">添加物品</span>
          </div>
          <el-form label-width="110px" :model="form" :inline="true">
            <el-form-item label="物品名">
              <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="在库起始时间">
              <el-date-picker v-model="time" type="datetimerange" :picker-options="pickerOptions"
                              range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期" align="right" value-format="yyyy-MM-dd HH:mm:ss">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="抵押人姓名">
              <el-input v-model="form.owner"></el-input>
            </el-form-item>
            <el-form-item label="抵押人联系方式">
              <el-input v-model="form.phone"></el-input>
            </el-form-item>
            <el-form-item label="备注">
              <el-input v-model="form.content" type="textarea" :autosize="{ minRows: 2, maxRows: 5}" style="width: 715px;"></el-input>
            </el-form-item>
            <div style="clear:both"></div>
            <el-form-item style="padding-left: 400px;">
              <el-button type="primary" @click="check()">提 交</el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'InWarehouse',
  data () {
    return {
      form: {
        name: '',
        owner: '',
        phone: '',
        content: ''
      },
      time: [],
      pickerOptions: {
        shortcuts: [{
          text: '未来一周',
          onClick (picker) {
            const start = new Date()
            const end = new Date()
            end.setTime(end.getTime() + 3600 * 1000 * 24 * 7)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '未来一个月',
          onClick (picker) {
            const start = new Date()
            const end = new Date()
            end.setTime(end.getTime() + 3600 * 1000 * 24 * 30)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '未来三个月',
          onClick (picker) {
            const start = new Date()
            const end = new Date()
            end.setTime(end.getTime() + 3600 * 1000 * 24 * 90)
            picker.$emit('pick', [start, end])
          }
        }]
      }
    }
  },
  mounted () {
  },
  methods: {
    loadData () {
    },
    check () {
      this.$confirm('此操作将入库该商品, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.submit()
        this.$message({
          type: 'success',
          message: '商品入库成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消入库'
        })
      })
    },
    submit () {
    }
  }
}
</script>

<style scoped>

</style>
