<template>
  <div>
    <div align="center" v-if="showAll">
      <el-input v-model="search" maxLength="25" size="medium" style="width: 300px" suffix-icon="el-icon-search" placeholder="输入物品名关键字搜索"/>
    </div>
    <div style="padding-top: 10px;" v-if="showAll">
      <el-row :gutter="20">
        <el-col :span="12" :offset="2">
          <el-card class="box-card" style="width: 900px;">
            <el-table :data="goodsData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))" style="width: 100%">
              <el-table-column prop="name" label="物品名" align="center"></el-table-column>
              <el-table-column prop="owner" label="抵押人" align="center"></el-table-column>
              <el-table-column prop="status" label="状态" align="center">
                <template slot-scope="scope">
                  <el-tag v-if="scope.row.status === 2">入库</el-tag>
                  <el-tag v-if="scope.row.status === 1" type="success">在库</el-tag>
                  <el-tag v-if="scope.row.status === 0" type="danger">出库</el-tag>
                </template>
              </el-table-column>
              <el-table-column label="操作" align="center">
                <template slot-scope="scope">
                  <el-button size="medium" type="primary" icon="el-icon-view" circle @click="handleDetail(scope.row)"></el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-card>
        </el-col>
      </el-row>
    </div>
<!--查看物品详情-->
    <div v-if="!showAll">
      <el-row :gutter="20">
        <el-col :span="12" :offset="2">
          <el-card class="box-card" style="width: 900px;">
            <div slot="header" class="clearfix">
              <span style="font-size: 16px;">物品信息</span>
              <el-button style="float: right;" size="mini" type="danger" plain icon="el-icon-back" @click="showAll = true">返回</el-button>
              <el-button size="mini" type="primary" plain icon="el-icon-view" @click="drawLine()">查看统计图表</el-button>
              <el-button size="mini" type="primary" plain icon="el-icon-view" @click="showImage()">查看摄像头图像</el-button>
            </div>
            <el-form label-width="110px" :model="correntGoods" :inline="true">
              <el-form-item label="物品名">
                <el-input v-model="correntGoods.name" disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="在库起始时间">
                <el-date-picker v-model="correntGoods.time" type="datetimerange" :picker-options="pickerOptions"
                                range-separator="至" start-placeholder="correntGoods.time[0]" end-placeholder="correntGoods.time[1]" align="right" value-format="yyyy-MM-dd HH:mm:ss" disabled="true">
                </el-date-picker>
              </el-form-item>
              <el-form-item label="抵押人姓名">
                <el-input v-model="correntGoods.owner" disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="抵押人联系方式">
                <el-input v-model="correntGoods.phone" disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="备注">
                <el-input v-model="correntGoods.content" type="textarea" :autosize="{ minRows: 2, maxRows: 5}" style="width: 715px;" disabled="true"></el-input>
              </el-form-item>
            </el-form>
          </el-card>
          <el-card class="box-card" style="width: 900px;">
            <div id="myChart" :style="{width: '300px', height: '300px'}"></div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShowAll',
  data () {
    return {
      search: '',
      showAll: true,
      correntGoods: {
        name: '钢卷',
        time: [
          '2019-11-15 00:44:53',
          '2020-02-13 00:44:53'
        ],
        owner: '张三',
        phone: '1234567890',
        content: '无'
      },
      goodsData: [
        {
          name: '钢卷',
          owner: '张三',
          status: 0
        },
        {
          name: '钢卷',
          owner: '张三',
          status: 1
        },
        {
          name: '钢卷',
          owner: '张三',
          status: 2
        }
      ]
    }
  },
  mounted () {
    this.loadAll()
  },
  methods: {
    loadAll () {
    },
    getGoods (goods) {
    },
    handleDetail (row) {
      // this.currentGoods = row
      this.getGoods(row)
      this.showAll = false
    },
    showImage () {
    },
    drawLine () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('myChart'))
      // 绘制图表
      myChart.setOption({
        title: { text: 'echart 使用实例' },
        tooltip: {},
        xAxis: {
          data: ['半径1', '半径2', '半径3', '直径1', '直径2', '直径3']
        },
        yAxis: {},
        series: [{
          name: '数量',
          type: 'bar',
          data: [5, 20, 36, 10, 10, 20]
        }]
      })
    },
    goBack () {
      this.showAll = true
    }
  }
}
</script>

<style scoped>

</style>
