<template>
  <div class="navbar">
    <!-- <hamburger id="hamburger-container" :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" /> -->
    <img class="logoing" src="../../assets/logo.png" @click="toggleClick">
    <p class="p1">营业中/已打烊</p>
    <img class="setimg" src="../../assets/set.png">
    <!-- <breadcrumb id="breadcrumb-container" class="breadcrumb-container" /> -->
    <a class="a1" @click="logout"><i class="el-icon-setting" />退出登录</a>
    <a class="a2"><i class="el-icon-setting" />账户设置</a>
    <div class="right-menu">
      <!-- <template v-if="device!=='mobile'">
        <router-link to="/profile/index">
            <i class="el-icon-setting"></i>账户设置
          </router-link>
        <a class="a1"><i class="el-icon-setting"></i>账户设置</a>
        <a class="a2"><i class="el-icon-setting"></i>退出登录</a>
      </template> -->

      <!-- <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar">
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/profile/index">
            <el-dropdown-item>Profile</el-dropdown-item>
          </router-link>
          <router-link to="/">
            <el-dropdown-item>打样</el-dropdown-item>
          </router-link>
          <a target="_blank" href="https://github.com/PanJiaChen/vue-element-admin/">
            <el-dropdown-item>Github</el-dropdown-item>
          </a>
          <a target="_blank" href="https://panjiachen.github.io/vue-element-admin-site/#/">
            <el-dropdown-item>Docs</el-dropdown-item>
          </a>
          <el-dropdown-item divided @click.native="logout">
            <span style="display:block;">Log Out</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown> -->
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'
import ErrorLog from '@/components/ErrorLog'
import Screenfull from '@/components/Screenfull'
import SizeSelect from '@/components/SizeSelect'
import Search from '@/components/HeaderSearch'

export default {
  components: {
    Breadcrumb,
    Hamburger,
    ErrorLog,
    Screenfull,
    SizeSelect,
    Search
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'avatar',
      'device'
    ])
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar')
    },
    async logout() {
      console.log(111111)
      await this.$store.dispatch('user/logout')
      this.$router.push(`/login`)
    },
    toggleClick() {
      console.log(11111)
      this.$store.dispatch('app/toggleSideBar')
      // this.$emit('toggleClick')
    }
  }
}
</script>

<style lang="scss" scoped>
.navbar {
  height: 80px;
  overflow: hidden;
  position: relative;
  // background: #fff;
  box-shadow: 0 1px 4px rgba(0,21,41,.08);
  background-color: #7764CA;
  color: #fff;
  .logoing{
    float: left;
    margin: 13px 0px 0px 20px;
  }
  .p1{
    float: left;
    color: #fff;
    font-size: 18px;
    line-height: 45px;
    font-weight: 700;
    margin-left: 30px;
    }
    .a1 , .a2{
      float: right;
      color: #fff;
      font-size: 16px;
      line-height: 80px;
      margin-right: 20px;
    }
    .setimg{
        width: 15px;
        height: 15px;
        float: left;
        margin: 33px 0px 0px 12px;
    }
  .hamburger-container {
    line-height: 80px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background .3s;
    -webkit-tap-highlight-color:transparent;

    &:hover {
      background: rgba(0, 0, 0, .025)
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 80px;
    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        margin-top: 5px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
