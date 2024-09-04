<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
import { useClipboard } from '@vueuse/core'
import { Message } from '@arco-design/web-vue'

import other from './json/other.json'

const { text, isSupported, copy } = useClipboard()

var value2 = ref("/w time3")

const value = computed(() => {
  return `${value2.value}`
})
const options = reactive(other)
const message = Message

function copyvalue() {
  copy(value.value)
  if (isSupported) {
    message.success(`已复制${value.value}`)
  }
}
const send: any = inject("send")

const showNotice = ref(true)
const noticeContent = '点击左下角‘指令生成’按钮切换不同页面 ||| 半夏公益服及其他任何衍生工具都是免费软件，如果你是付费购买的，那你就被骗了，请及时退款并举报。'

// 在页面加载时设置一个延时，用于显示滚动公告，你可以根据需求调整延时时长
onMounted(() => {
  setTimeout(() => {
    showNotice.value = true
  }, 1000)
})
</script>

<template>
  <div class="commuse">
    <!-- 滚动公告 -->
    <div class="scrolling-notice" v-if="showNotice">
      <marquee behavior="scroll" direction="left">{{ noticeContent }}</marquee>
    </div>
    <div class="title"> 常用指令 </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 选择指令: </div>
      <a-cascader allow-search v-model="value2" :options="options" placeholder="" filterable />
    </div>

    <div class="generate">
      <a-input v-model="value" placeholder="" />
      <a-button type="outline" @click="copyvalue">复制</a-button>
      <!-- <a-button type="outline" @click="copyvalue">执行</a-button> -->
    </div>
  </div>
</template>

<style lang="less" scoped>
/* 添加样式以美化滚动公告 */
.scrolling-notice {
  color: #BEBEBE;
  padding: 8px;
  font-size: 14px;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  border-radius: 10px;
  /* 添加圆角样式 */
}

.commuse {
  width: 100%; // 使用百分比宽度，使其自适应
  margin: auto;
  padding: 0 10px; // 添加内边距，确保在小屏幕上也有足够的间距

  @media (max-width: 768px) {
    // 当屏幕宽度小于768px时
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

  @media (max-width: 768px) {
    // 在小屏幕上改为垂直排列
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>