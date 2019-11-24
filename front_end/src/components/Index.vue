<template>
  <el-container style="border: 0 solid #eee">
    <el-header style="padding-left: 0;">
      <section class="text-white tm-font-big tm-parallax">
        <nav class="navbar navbar-expand-md tm-navbar scroll" id="tmNav">
          <div class="container">
            <div class="tm-next">
              <img src="../../public/img/logo.png" width="45px">
              <a class="navbar-brand" @click="toIndex()">仓储管理系统</a>
            </div>
<!--上方导航栏-->
<!--            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">-->
<!--              <i class="fas fa-bars navbar-toggler-icon"></i>-->
<!--            </button>-->
<!--            <div class="collapse navbar-collapse" id="navbarSupportedContent">-->
<!--              <ul class="navbar-nav ml-auto">-->
<!--                <li class="nav-item">-->
<!--                  <a class="nav-link tm-nav-link" @click="toPage1(1)" style="font-size: 20px;">小区资讯</a>-->
<!--                </li>-->
<!--                <li class="nav-item">-->
<!--                  <a class="nav-link tm-nav-link" @click="toPage1(2)" style="font-size: 20px;">小区团购</a>-->
<!--                </li>-->
<!--                <li class="nav-item">-->
<!--                  <el-dropdown @command="toPage1">-->
<!--                    <span class="nav-link tm-nav-link" style="font-size: 20px;">-->
<!--                      <i class="el-icon-arrow-down el-icon&#45;&#45;right" ></i>小区服务-->
<!--                    </span>-->
<!--                    <el-dropdown-menu slot="dropdown">-->
<!--                      <el-dropdown-item command="4" style="font-size: 14px;">超市送货</el-dropdown-item>-->
<!--                      <el-dropdown-item command="5" style="font-size: 14px;">电脑维修</el-dropdown-item>-->
<!--                      <el-dropdown-item command="6" style="font-size: 14px;">物业维修</el-dropdown-item>-->
<!--                    </el-dropdown-menu>-->
<!--                  </el-dropdown>-->
<!--                </li>-->
<!--                <li class="nav-item">-->
<!--                  <a class="nav-link tm-nav-link" @click="toPage1(3)" style="font-size: 20px;">小区论坛</a>-->
<!--                </li>-->
<!--              </ul>-->
<!--            </div>-->
          </div>
        </nav>
      </section>
    </el-header>
    <el-container style="padding-top: 20px;">
      <el-aside width="150px" style="background-color: rgb(238, 241, 246)">
        <el-menu default-active="1" style="height:100vh;width:150px;float:left;overflow-y:auto;">
          <div align="center" style="padding-bottom: 15px;">
            <i class="fas fa-3x fa-user-circle text-center tm-icon"></i>
            <div style="clear:both"></div>
            <el-tooltip class="item" effect="light" content="点击退出登录" placement="right">
              <el-button id="logout" type="primary" plain size="mini" icon="el-icon-info" @click="logout()">管理员</el-button>
            </el-tooltip>
            <div style="clear:both"></div>
          </div>
          <el-menu-item index="1" @click="toPage(1, 1)">
            <i class="el-icon-folder-add"></i>
            <span slot="title" style="font-size: 16px">新增入库</span>
          </el-menu-item>
          <el-menu-item index="2" @click="toPage(2, 2)">
            <i class="el-icon-folder-opened"></i>
            <span slot="title" style="font-size: 16px">查看全部</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-main>
        <keep-alive>
          <component :is="tabView"></component>
        </keep-alive>
      </el-main>
    </el-container>
    <el-dialog title="警报" :visible.sync="dialogFormVisible" width="400px">
      <el-form >
        <el-form-item label="设备" >
          {{data.id}}
        </el-form-item>
        <el-form-item label="情况">
          {{data.alertType}}
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="handleDetailFinish()">确定</el-button>
      </div>
    </el-dialog>
  </el-container>
</template>

<script>
import page1 from './InWarehouse.vue'
import page2 from './ShowAll.vue'
export default {
  name: 'Index',
  data () {
    return {
      tabView: 'page1',
      openList: ['1'],
      dialogFormVisible: false,
      message: 'a',
      data: ''
    }
  },
  methods: {
    handleDetailFinish () {
      this.dialogFormVisible = false
    },
    toIndex () {
      // let bodyFormData = new FormData()
      // let url = '/user-server/api/user/logout'
      // this.$axios({
      //   method: 'post',
      //   url: url,
      //   data: bodyFormData,
      //   config: { headers: { 'Content-type': 'multipart/form-data' } } }
      // ).then(response => {
      //   if (response.data.logout === 1) {
      //     sessionStorage.clear()
      //     this.$router.push({ name: 'home' })
      //   } else {
      //     this.$alert('前往首页失败！请关闭页面重试')
      //   }
      // })
    },
    logout () {
      // let bodyFormData = new FormData()
      // let url = '/user-server/api/user/logout'
      // this.$axios({
      //   method: 'post',
      //   url: url,
      //   data: bodyFormData,
      //   config: { headers: { 'Content-type': 'multipart/form-data' } } }
      // ).then(response => {
      //   if (response.data.logout === 1) {
      //     sessionStorage.clear()
      //     this.$router.push({ name: 'Login' })
      //   } else {
      //     this.$alert('退出登录失败！请关闭页面重试')
      //   }
      // })
    },
    // toPage1 (id) {
    //   this.openList[0] = '1'
    //   this.tabView = `page${id}`
    // },
    toPage (id1, id2) {
      this.openList[0] = id1
      this.tabView = `page${id2}`
    },
    loadData () {
      // this.userInfo.username = sessionStorage.getItem('username')
      // if (this.userInfo.username === '' || this.userInfo.username === null) {
      //   this.$router.push({ name: 'Login' })
      // }
      // this.userInfo.phone = sessionStorage.getItem('phone')
      // this.userInfo.communityId = sessionStorage.getItem('communityId')
      // this.userInfo.email = sessionStorage.getItem('email')
      // let bodyFormData = new FormData()
      // bodyFormData.set('id', this.userInfo.communityId)
      // let url = '/user-server/api/user/getCommunityById'
      // this.$axios({
      //   method: 'post',
      //   url: url,
      //   data: bodyFormData,
      //   config: { headers: { 'Content-type': 'multipart/form-data' } } }
      // ).then(response => {
      //   this.userInfo.community = response.data.community
      //   sessionStorage.setItem('communityAddress', response.data.address)
      //   sessionStorage.setItem('community', this.userInfo.community)
      // })
    },
    exit () {
      /* eslint-disable */
      window.is_confirm = false
      // 关闭窗口时弹出确认提示
      $(window).bind('beforeunload', function () {
        // 只有在标识变量is_confirm不为false时，才弹出确认提示
        if (window.is_confirm !== false) {
          window.document.getElementById('logout').click()
          return '您可能有数据没有保存'
        }
      })
      // mouseleave mouseover事件也可以注册在body、外层容器等元素上
        .bind('mouseover mouseleave', function (event) {
          window.is_confirm = (event.type === 'mouseleave')
        })
    }
  },
  components: {
    page1,
    page2
  },
  mounted () {
    this.loadData()
    this.exit()
    let that = this
    this.message = 'c'
    const source = new EventSource('/stream-sse/connecttime')
    source.onopen = function () {
      that.message = 'connected'
      window.console.log(that.message)
    }

    source.addEventListener('connecttime', function (event) {
      // 测试成功，稳定sse时使用窗口
      that.dialogFormVisible = true
      that.data = event.data
      that.data = JSON.parse(that.data)
      window.console.log(that.data)
    }, false)
  }
}
</script>

<style scoped>

</style>
