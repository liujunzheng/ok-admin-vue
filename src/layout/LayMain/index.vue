<!-- 
  * @describe: 主内容区组件
-->

<template>
  <router-view v-slot="{ Component, route }">
    <transition appear :name="route.meta.transition || 'fade'" mode="out-in">
      <keep-alive v-if="keepAlives" :include="keepAlives">
        <component :is="Component" :key="route.fullPath" />
      </keep-alive>
      <component :is="Component" v-else :key="route.fullPath" />
    </transition>
  </router-view>
</template>
<script lang="ts">
  import { defineComponent } from 'vue'
  import useAdminStore from '@/store/adminStore'
  export default defineComponent({
    name: 'LayMain',
    setup() {
      const adminStore = useAdminStore()
      const keepAlives = adminStore.keepAlivesGetter

      return { keepAlives }
    }
  })
</script>
<style lang="scss" scoped></style>
