
<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
import { useClipboard } from '@vueuse/core'
import { Message } from '@arco-design/web-vue'
import { useAppStore } from '@/store/modules/app'

import monster from './json/monster.json'
const { text, isSupported, copy } = useClipboard()
const appStore = useAppStore()

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
.commuse {
  width: 500px;
  margin: auto;
}

.commuse-item {
  display: flex;
  align-items: center;
  color: #000;
  margin: 18px 0;

  >div {
    &:nth-child(1) {
      width: 150px;
      text-align: right;
      padding-right: 10px;
    }
  }
}

.generate {
  display: flex;
  align-items: center;
  margin-left: 100px;
}
</style>
