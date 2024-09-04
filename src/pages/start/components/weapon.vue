<template>
  <div>
    <!-- 滚动公告 -->
    <div class="scrolling-notice" v-if="showNotice">
      <marquee behavior="scroll" direction="left">{{ noticeContent }}</marquee>
    </div>

    <!-- 原有的组件内容 -->
    <div class="commuse">
      <div class="commuse-item">
        <div class="text-slate-900 dark:text-slate-100"> 武器: </div>
        <a-cascader allow-search v-model="value2" :options="options" placeholder="请输入物品" filterable />
      </div>
      <div class="commuse-item">
        <div class="text-slate-900 dark:text-slate-100"> 数量: </div>
        <a-input-number v-model="num" placeholder="" mode="button" size="large" class="input-demo" />
      </div>
      <div class="commuse-item">
        <div class="text-slate-900 dark:text-slate-100"> 等级: </div>
        <a-input-number v-model="grade" placeholder="请输入" mode="button" size="large" class="input-demo" />
      </div>
      <div class="commuse-item">
        <div class="text-slate-900 dark:text-slate-100"> 精炼等级: </div>
        <a-input-number v-model="refined" placeholder="请输入" mode="button" size="large" class="input-demo" />
      </div>
      <div class="generate">
        <a-input v-model="value" placeholder="" />
        <a-button type="outline" @click="copyvalue">复制</a-button>
        <a-button type="outline" v-if="appStore.isLogin" @click="send(value)">执行</a-button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed, inject, onMounted } from 'vue'
import { useClipboard } from '@vueuse/core'
import weapon from './json/weapon.json'
import { useAppStore } from '@/store/modules/app'
import { Message } from '@arco-design/web-vue'

const { text, isSupported, copy } = useClipboard()
const appStore = useAppStore()

const value2 = ref(23013)
const num = ref(1)
const grade = ref(90)
const refined = ref(5)

const value = computed(() => {
  return `/give ${value2.value} x${num.value} lv${grade.value} r${refined.value}`
})

const options = reactive(weapon)
const message = Message

function copyvalue() {
  copy(value.value)
  if (isSupported) {
    message.success(`已复制${value.value}`)
  }
}

const send: any = inject("send")

const showNotice = ref(true)
const noticeContent = '半夏公益服及其他任何衍生工具都是免费软件，如果你是付费购买的，那你就被骗了，请及时退款并举报。'

// 在页面加载时设置一个延时，用于显示滚动公告，你可以根据需求调整延时时长
onMounted(() => {
  setTimeout(() => {
    showNotice.value = true
  }, 1000)
})
</script>

<style lang="less" scoped>
/* 添加样式以美化滚动公告 */
.scrolling-notice {
  color: #BEBEBE;
  padding: 8px;
  font-size: 14px;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  border-radius: 10px; /* 添加圆角样式 */
}
.commuse {
  width: 100%; // 使用百分比宽度，使其自适应
  margin: auto;
  padding: 0 10px; // 添加内边距，确保在小屏幕上也有足够的间距

  @media (max-width: 768px) { // 当屏幕宽度小于768px时
    flex-direction: column; // 改为垂直方向排列
    align-items: flex-start; // 对齐到左侧
  }
}

.commuse-item {
  display: flex;
  flex-direction: column; // 垂直排列
  align-items: center; // 居中对齐
  color: #000;
  margin: 20px 0; // 增加上下间距
}

.label {
  width: 100%; // 标签占据全部宽度
  padding-bottom: 5px; // 下方添加一些间距
}

.input {
  width: 100%; // 输入框占据全部宽度
}

.generate {
  display: flex;
  align-items: center;
  margin-left: 0; // 移除左边距
  justify-content: space-between; // 均匀分布按钮和输入框

  @media (max-width: 768px) { // 在小屏幕上改为垂直排列
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>
