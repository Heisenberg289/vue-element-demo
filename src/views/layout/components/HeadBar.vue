<template>
  <div class="head-bar">
    <div class="head-left">
      <div class="head-logo">logo</div>
      <div v-for="(item, index) in permission_routers" v-if="item.meta" :key="index" class="head-link" @click="setUrl(item)">
        {{ generateTitle(item.meta.title) }}
      </div>
    </div>
    <div class="head-right">
      <template v-if="device!=='mobile'">
        <!--<search class="right-menu-item" />-->

        <!--<error-log class="errLog-container right-menu-item hover-effect"/>-->

        <el-tooltip :content="$t('navbar.screenfull')" effect="dark" placement="bottom">
          <screenfull class="right-menu-item hover-effect"/>
        </el-tooltip>

        <!--<el-tooltip :content="$t('navbar.size')" effect="dark" placement="bottom">-->
          <!--<size-select class="right-menu-item hover-effect"/>-->
        <!--</el-tooltip>-->

        <lang-select class="right-menu-item hover-effect"/>

        <!--<el-tooltip :content="$t('navbar.theme')" effect="dark" placement="bottom">-->
          <!--<theme-picker class="right-menu-item hover-effect"/>-->
        <!--</el-tooltip>-->
      </template>
      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar">
          <i class="el-icon-caret-bottom"/>
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/">
            <el-dropdown-item>
              {{ $t('navbar.dashboard') }}
            </el-dropdown-item>
          </router-link>
          <el-dropdown-item divided>
            <span style="display:block;" @click="logout">{{ $t('navbar.logOut') }}</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import ScrollPane from '@/components/ScrollPane'
import { generateTitle } from '@/utils/i18n'
import { mapGetters } from 'vuex'
import ErrorLog from '@/components/ErrorLog'
import Screenfull from '@/components/Screenfull'
import SizeSelect from '@/components/SizeSelect'
import LangSelect from '@/components/LangSelect'
import ThemePicker from '@/components/ThemePicker'
import Search from '@/components/HeaderSearch'

export default {
  components: {
    ScrollPane,
    ErrorLog,
    Screenfull,
    SizeSelect,
    LangSelect,
    ThemePicker,
    Search
  },
  data() {
    return {
      visible: false,
      top: 0,
      left: 0,
      selectedTag: {}
    }
  },
  computed: {
    ...mapGetters([
      'permission_routers',
      'sidebar',
      'name',
      'avatar',
      'device'
    ])
  },
  watch: {
    $route() {
      console.log(this.$router)
    }
  },
  mounted() {
    console.log(this.permission_routers)
  },
  methods: {
    setUrl(item) {
      const newUrl = this.permission_routers.filter(val => val.path === item.path)
      this.$store.commit('SET_CHOSED_ROUTERS', newUrl)
    },
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        location.reload()// In order to re-instantiate the vue-router object to avoid bugs
      })
    },
    generateTitle
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .head-bar{
    background-color: #041c40;
    color: rgb(191, 203, 217);
    align-items: center;
    height: 60px;
    z-index: 9;
    font-size: 14px;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    .head-left{
      display: flex;
    }
    .head-right{
      display: flex;
      align-items: center;
      &>div{
        cursor: pointer;
        padding: 0 8px;
        height: 100%;
        font-size: 18px;
      }
      .user-avatar{
        cursor: pointer;
        width: 40px;
        height: 40px;
        border-radius: 10px;
      }
    }
    .head-logo{
      display: flex;
      width: 200px;
      height: 100%;
      border: 1px solid red;
      align-items: center;
      justify-content: center;
    }
    .head-link{
      margin: 0 10px;
      cursor: pointer;
    }
  }
</style>

<style rel="stylesheet/scss" lang="scss">
</style>
