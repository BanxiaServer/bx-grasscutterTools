<script setup lang="ts">
import { reactive, ref, onMounted, watch } from 'vue'
import {
  IconMenuFold,
  IconMenuUnfold,
  IconApps,
  IconBug,
  IconBulb,
} from '@arco-design/web-vue/es/icon';
import router from "@/router/index"
import { useAppStore } from '@/store'
import { Message } from '@arco-design/web-vue';

const appStore = useAppStore()
const datav = reactive([
  { name: '快捷指令', path: "/start/other" },
  { name: '圣遗物', path: "/start/holyrelic" },
  { name: '怪物生成', path: "/start/monster" },
  { name: '物品', path: "/start/thing" },
  { name: '角色', path: "/start/people" },
  { name: '武器', path: "/start/weapon" },
  { name: '角色属性', path: "/start/role" },
])

const GMTitle = ref("GM控制面板")

function topath(path: string) {
  router.push({ path: path })
}

const selectedKey = ref([""])
onMounted(() => {
  selectedKey.value = [router.currentRoute.value.fullPath]
})

watch(
  () => appStore.isLogin,
  () => {
    const isLogin: boolean = appStore.isLogin
    if (isLogin) {
      GMTitle.value = "GM控制面板-已登录"
    } else {
      GMTitle.value = "GM控制面板"
    }
  },
  {
    immediate: true,
  },
)

watch(
  () => router.currentRoute.value.path,
  (newValue, oldValue) => {
    selectedKey.value = [newValue]
  },
  { immediate: true }
)
</script>

<template>
  <div class="nav">
    <a-menu
      mode="horizontal"
      showCollapseButton
      :default-open-keys="['0']"
      :selected-keys="selectedKey"
    >
      <a-sub-menu key="0">
        <template #icon>
          <IconApps></IconApps>
        </template>
        <template #title>指令生成</template>
        <a-menu-item v-for="(item, index) in datav" :key="item.path" @click="topath(item.path)">
          {{ item.name }}
        </a-menu-item>
      </a-sub-menu>
    </a-menu>
  </div>
</template>

<style lang="less" scoped>
.nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background-color: #fff;
  box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.1);
  padding: 10px 20px;
  display: flex;
  align-items: center;
  justify-content: center; /* 将 justify-content 设置为 center 来居中对齐 */
  height: 57px;
}

.nav > div {
  display: flex;
  justify-content: center; /* 确保内部内容也居中对齐 */
  width: 100%;
}
</style>
