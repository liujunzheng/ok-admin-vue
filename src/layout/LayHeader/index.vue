<template>
  <div class="lay-header lay-select-none">
    <n-grid x-gap="2" cols="2">
      <!-- left -->
      <n-gi>
        <div class="header-left">
          <!-- 菜单切换 -->
          <button-menu />

          <!-- 刷新页面 -->
          <button-refresh />

          <!-- 面包屑 -->
          <n-breadcrumb sm-hidden xs-hidden>
            <n-breadcrumb-item v-for="item in matcheds" :key="item">{{ item }}</n-breadcrumb-item>
          </n-breadcrumb>
        </div>
      </n-gi>

      <!-- right -->
      <n-gi suffix>
        <div class="header-right">
          <!-- 锁屏 -->
          <div title="锁屏" xs-hidden class="flex-center btn-content lay-hover">
            <n-icon class="lay-hover" size="20">
              <lock-closed />
            </n-icon>
          </div>

          <!-- 全屏按钮 -->
          <button-full-screen xs-hidden size="18" />

          <!-- 语言切换 -->
          <n-dropdown trigger="hover" :options="optionsISO" @select="handleSelect">
            <div title="语言切换" xs-hidden class="flex-center btn-content lay-hover">
              <n-icon class="lay-hover" size="18">
                <globe-outline />
              </n-icon>
            </div>
          </n-dropdown>

          <!-- 用户菜单 -->
          <user-menu />

          <!-- 系统设置 -->
          <div title="系统设置" class="flex-center btn-content lay-hover" @click="setShow = true">
            <n-icon class="setting-btn lay-hover" size="18">
              <settings-outline />
            </n-icon>
            <lay-setting v-model:show="setShow"></lay-setting>
          </div>
        </div>
      </n-gi>
    </n-grid>
  </div>
</template>

<script lang="ts">
  import { useRoute } from 'vue-router'
  import { defineComponent, computed, ref, watchEffect } from 'vue'
  import { GlobeOutline, SettingsOutline } from '@vicons/ionicons5'
  import { NGi, NGrid, NBreadcrumb, NBreadcrumbItem, NIcon, NDropdown } from 'naive-ui'
  import useThemeStore from '@/store/themeStore'
  import { LockClosed } from '@/icon/material-icon/index'
  import { ButtonFullScreen, ButtonMenu, ButtonRefresh, UserMenu } from './components/index'
  import LaySetting from '@/layout/LaySetting/index.vue'

  export default defineComponent({
    name: 'LayHeader',
    components: {
      NGi,
      NGrid,
      NIcon,
      NBreadcrumb,
      NBreadcrumbItem,
      NDropdown,
      LaySetting,
      GlobeOutline,
      SettingsOutline,
      UserMenu,
      LockClosed,
      ButtonMenu,
      ButtonRefresh,
      ButtonFullScreen
    },
    setup() {
      const themeStore = useThemeStore()
      const route = useRoute()
      const setShow = ref(false)
      const hoverColor = computed(() => {
        return themeStore.isDarkThemeGetter ? '#101014' : '#f8f8f9'
      })
      const matcheds = ref([] as Array<string>)

      watchEffect(() => {
        // 面包屑
        matcheds.value = []
        const matched = route.matched
        for (let i = 0; i < matched.length; i++) {
          matcheds.value.push(matched[i].meta.title as string)
        }
      })

      return {
        setShow,
        matcheds,
        hoverColor,
        optionsISO: [
          {
            label: '简体中文',
            key: 'zh'
          },
          {
            label: 'English',
            key: 'en'
          }
        ],
        handleSelect: (val: any) => {
          console.log(val)
        }
      }
    }
  })
</script>

<style lang="scss" scoped>
  .lay-header {
    height: 100%;
    display: flex;
    align-items: center;
  }
  .flex-center {
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }

  .header-left {
    display: flex;
    padding: 0 6px;
    align-items: center;
  }
  .header-right {
    display: flex;
    padding: 0 6px;
    align-items: center;
    justify-content: flex-end;
  }
  .btn-content {
    height: 64px;
    box-sizing: border-box;
    transition: all 0.25s;
    padding: 0 12px;
    &:hover {
      background: v-bind(hoverColor);
    }
  }
</style>
