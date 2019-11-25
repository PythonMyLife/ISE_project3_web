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
              <el-table-column prop="pledgeId" label="抵押人" align="center"></el-table-column>
              <el-table-column prop="status" label="状态" align="center">
                <template slot-scope="scope">
                  <el-tag v-if="scope.row.logicalState === 'INWAREHOUSING'">入库</el-tag>
                  <el-tag v-if="scope.row.logicalState === 'INWAREHOUSED'" type="success">在库</el-tag>
                  <el-tag v-if="scope.row.logicalState === 'EXWAREHOUSING'" type="danger">出库</el-tag>
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
                                range-separator="至" start-placeholder="correntGoods.warehouseInTime[0]" end-placeholder="correntGoods.warehouseInTime[1]" align="right" value-format="yyyy-MM-dd HH:mm:ss" disabled="true">
                </el-date-picker>
              </el-form-item>
              <el-form-item label="物理状态">
                <el-input v-model="correntGoods.physicalState" disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="位置信息">
                <el-input v-model="correntGoods.location.locationId" disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="备注">
                <el-input v-model="correntGoods.content" type="textarea" :autosize="{ minRows: 2, maxRows: 5}" style="width: 715px;" disabled="true"></el-input>
              </el-form-item>
            </el-form>
          </el-card>
          <el-card class="box-card" style="width: 900px;">
            <div id="myChart" :style="{width: '700px', height: '300px'}"></div>
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
      let bodyFormData = new FormData()
      let url = '/pledge/getAll'
      this.$axios({
        method: 'post',
        url: url,
        data: bodyFormData,
        config: { headers: { 'Content-type': 'multipart/form-data' } } }
      ).then(response => {
        console.log(response.data)
        this.goodsData = response.data
      })
    },
    getGoods (goods) {
      let url = '/pledge/getOne'
      this.$axios.get(url, {
        params: {
          pledgeID: goods.pledgeId
        }
      }).then(response => {
        console.log(response.data)
        this.correntGoods = response.data
      })
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
        tooltip: {},
        xAxis: {
          name: '日期',
          nameLocation: 'end',
          nameTextStyle: {
            padding: -24
          },
          data: ['2018-12-01 01:00:00', '2018-12-01 02:00:00', '2018-12-01 03:00:00', '2018-12-01 04:00:00', '2018-12-01 05:00:00'],
          type: 'category',
          boundaryGap: false
        },
        grid: [{
          left: '20%',
          bottom: '20%',
          top: '20%',
          right: '0%'
        }],
        dataZoom: [
          { // 这个dataZoom组件，默认控制x轴。
            type: 'slider', // 这个 dataZoom 组件是 slider 型 dataZoom 组件
            start: 0, // 左边在 0% 的位置。
            end: 20 // 右边在 30% 的位置。
          }
        ],
        color: ['#dcb7fe'],
        yAxis: {
          type: 'value',
          name: '分数',
          nameLocation: 'end',
          nameTextStyle: {
            padding: -8
          },
          axisLabel: {
            formatter: function (value) {
              var texts = []
              if (value === 1) {
                texts.push('入库中')
              } else if (value === 2) {
                texts.push('已入库')
              } else if (value === 3) {
                texts.push('出库中')
              } else if (value === 4) {
                texts.push('已出库')
              } else if (value === 5) {
                texts.push('V')
              } else if (value === 6) {
                texts.push('劣V')
              } else if (value === 0) {
                texts.push('断流')
              }
              return texts
            }
          }
        },
        series: [
          {
            data: [1, 2, 3, 4, 5],
            type: 'line'
          }
        ]
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
