
<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
import { useClipboard } from '@vueuse/core'
import { Message } from '@arco-design/web-vue'
import { useAppStore } from '@/store/modules/app'

import monster from './json/monster.json'
const { text, isSupported, copy } = useClipboard()
const appStore = useAppStore()

function customLabel({ labels }: any) {
  return labels.join(' / ').length > 5 ? labels.join(' / ').slice(0, 5) + '...' : labels.join(' / ');
}

var value2 = ref(21010101)
var num = ref(-1)
var grade = ref(-1)
var hp = ref(-1)
var maxhp = ref(-1)
var atk = ref(-1)
var def = ref(-1)

const value = computed(() => {
  let result = `/spawn ${value2.value}`
  if (num.value !== -1) {
    result += ` x${num.value}`
  }
  if (grade.value !== -1) {
    result += ` lv${grade.value}`
  }
  if (hp.value !== -1) {
    result += ` hp${hp.value}`
  }
  if (maxhp.value !== -1) {
    result += ` maxhp${maxhp.value}`
  }
  if (atk.value !== -1) {
    result += ` atk${atk.value}`
  }
  if (def.value !== -1) {
    result += ` def${def.value}`
  }
  return result
})
const options = reactive(monster)
const message = Message

function copyvalue() {
  copy(value.value)
  if (isSupported) {
    message.success(`已复制${value.value}`)
  }
}
const send: any = inject("send")
</script>
<template>
  <div class="commuse">
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 怪物: </div>
      <a-cascader allow-search v-model="value2" :options="options" placeholder="" filterable />
    </div>

    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 数量: </div>
      <a-input-number v-model="num" placeholder="请输入数量" mode="button" size="large" class="input-demo" />
    </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 等级: </div>
      <a-input-number v-model="grade" placeholder="请输入等级" mode="button" size="large" class="input-demo" />
    </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 血量: </div>
      <a-input-number v-model="hp" placeholder="请输入数值" mode="button" size="large" class="input-demo" />
    </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 最大血量: </div>
      <a-input-number v-model="maxhp" placeholder="请输入数值" mode="button" size="large" class="input-demo" />
    </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 攻击力: </div>
      <a-input-number v-model="atk" placeholder="请输入数值" mode="button" size="large" class="input-demo" />
    </div>
    <div class="commuse-item">
      <div class="text-slate-900 dark:text-slate-100"> 防御力: </div>
      <a-input-number v-model="def" placeholder="请输入数值" mode="button" size="large" class="input-demo" />
    </div>

    <div class="generate">
      <a-input v-model="value" placeholder="" />
      <a-button type="outline" @click="copyvalue">复制</a-button>
      <a-button type="outline" v-if="appStore.isLogin" @click="send(value)">执行</a-button>
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