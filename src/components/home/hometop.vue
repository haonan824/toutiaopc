<template>
  <div>
    <el-row style="height:60px" type="flex" align="middle" justify="space-between">
      <el-col :span="7">
        <i @click="getclick" :class="variable ? 'el-icon-s-unfold' : 'el-icon-s-fold'" style="margin-right:5px"></i>
        <span>江苏传智播客教育科技股份有限公司</span>
      </el-col>
      <el-col :span="4">
        <el-row  type="flex" align="middle">
            <el-avatar :src="userInfo.photo ? userInfo.photo : defaultImg" class="via"></el-avatar>
            <el-dropdown  @command='handle'>
            <span class="el-dropdown-link">
                {{userInfo.name}}
                <i class="el-icon-arrow-down el-icon--right"></i>
             </span>
            <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command='init'>个人详细</el-dropdown-item>
                <el-dropdown-item command='git'>博客园</el-dropdown-item>
                <el-dropdown-item command='lgout'>退出</el-dropdown-item>
            </el-dropdown-menu>
            </el-dropdown>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import eventBus from '../../utils/eventBus'
export default {
  data () {
    return {
      variable: false,
      userInfo: {}, // 用户信息
      defaultImg: require('../../assets/img/login_dg.jpg')
    }
  },
  created () {
    this.getintro()
    eventBus.$on('Implementation', () => {
      this.getintro()
    })
  },
  methods: {
    getclick () {
      this.variable = !this.variable
      eventBus.$emit('transform')
    },
    getintro () {
      this.$axios({
        url: '/user/profile'
      }).then(res => {
      // console.log(res.data)
        this.userInfo = res.data
      })
    },
    handle (command) {
      if (command === 'lgout') {
        window.localStorage.removeItem('user-token')
        this.$router.push('/login')
      } else if (command === 'git') {
        window.location.href = 'https://www.cnblogs.com/haonanY/'
      } else if (command === 'init') {
        this.$router.push('/home/account')
      }
    }
  }
}
</script>

<style lang="less" scoped>
  .via{
      margin-right: 10px;
  }
</style>
