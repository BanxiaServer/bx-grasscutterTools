
<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
import { useClipboard } from '@vueuse/core'
import { Message } from '@arco-design/web-vue'
import { useAppStore } from '@/store/modules/app'

const { text, isSupported, copy } = useClipboard()
const appStore = useAppStore()

const options = reactive([
  {
    title: '加速*3倍',
    value: '/w time3',
  },
  {
    title: '点亮地图',
    value: '/prop unlockmap 1',
  },
  {
    title: '解锁所有状态',
    value: '/unlockall',
  },
  {
    title: '解锁风之翼',
    value: '/prop fly on',
  },
  {
    title: '无敌',
    value: '/prop god on',
  },
  {
    title: '无限体力',
    value: '/prop ns on',
  },
  {
    title: '无限能量',
    value: '/prop ue on',
  },
  {
    title: '无cd',
    value: '/setstats cdr 100%',
  },
  {
    title: '满血',
    value: '/h',
  },
  {
    title: '设置纪行等级',
    value: '/prop bp 50',
  },
  {
    title: '世界等级8',
    value: '/prop wl 8',
  },
  {
    title: '全队充能',
    value: '/spawn 2008 25',
  },
  {
    title: '查看坐标',
    value: '/pos',
  },
  {
    title: '一万纠缠',
    value: '/give 223 x10000',
  },
  {
    title: '击杀全部怪物',
    value: '/killall',
  },
  {
    title: '自杀',
    value: '/kill 0',
  },
  {
    title: '快速重进游戏',
    value: '/kick',
  },
  {
    title: '清空角色命座(需重登)',
    value: '/resetconst',
  },
  {
    title: '进入多人游戏状态',
    value: '/coop',
  },
  {
    title: '[多人游戏]全体传送',
    value: '/tpall',
  },
  {
    title: '起飞',
    value: '/tp ~ ~100 ~',
  },
])
const message = Message

function copyvalue(value: string) {
  copy(value)
  if (isSupported) {
    message.success(`已复制${value}`)
  }
}
const send: any = inject("send")
</script>

<template>
  <div class="commuse">
    <div v-for="(item, index) in options" :key="index">
      <div class="text-slate-900 dark:text-slate-100">{{ item.title }}</div>
      <div>
        <a-input v-model="item.value" placeholder="" disabled />
      </div>
      <div>
        <a-button type="outline" @click="copyvalue(item.value)">复制</a-button>
        <a-button type="outline" v-if="appStore.isLogin" @click="send(item.value)">执行</a-button>
      </div>
    </div>
  </div>
</template>
<style lang="less" scoped>
.commuse {
  width: 500px;
  margin: auto;

  >div {
    margin: 10px 0;
    display: flex;
    align-items: center;
    color: #000;

    >div {
      &:nth-child(1) {
        width: 130px;
      }

      margin: 0 5px;
    }
  }
}

.generate {
  display: flex;
  align-items: center;
  margin-left: 100px;
}
</style>
