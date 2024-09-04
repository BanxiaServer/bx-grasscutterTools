<template>
  <div class="personnel">
    <!-- 滚动公告 -->
    <div class="scrolling-notice" v-if="showNotice">
      <marquee behavior="scroll" direction="left">{{ noticeContent }}</marquee>
    </div>
    <a-space direction="vertical">
      <a-button type="outline" @click="updatePlayerList">刷新</a-button>
      <a-table :columns="columns" :data="appStore.PlayerList">
        <template #optional="{ record }">
          <a-button @click="handle(record)">操作</a-button>
        </template>
      </a-table>
    </a-space>
  </div>
  <a-drawer :width="500" :visible="visible" placement="right" @ok="handleOk" @cancel="handleCancel" unmountOnClose>
    <template #title>
      操作 UID @{{ handleUid }}
    </template>
    <div class="personnel-drawer">
      <a-space direction="vertical" fill>
        <div>
          一键操作
        </div>
        <a-space wrap>
          <a-button v-for="(item, index) in operationData" type="outline" :key="index" @click="handleOP(item.value)">{{
            item.label
          }}</a-button>
        </a-space>
      </a-space>
    </div>
  </a-drawer>
</template>
<script setup lang="ts">
import { reactive, ref, onMounted } from 'vue'
import { useAppStore } from '@/store/modules/app'
import operationdata from './operation.json'
const appStore = useAppStore()
function updatePlayerList() {
  appStore.socketSend("{\"type\":\"Player\",\"data\":\"0\"}");
}
const data = ref([])
const columns = [
  {
    title: 'UID',
    dataIndex: 'uid',
  },
  {
    title: '昵称',
    dataIndex: 'nickname',
  },
  {
    title: '签名',
    dataIndex: 'signature',
  },
  {
    title: '等级',
    dataIndex: 'Level',
  },
  {
    title: '坐标',
    dataIndex: 'pos',
  },
  {
    title: '场景',
    dataIndex: 'SceneId',
  },
  {
    title: '操作',
    slotName: 'optional'
  },
];

const operationData = ref(operationdata)
const handleUid = ref(1)
const handle = (record: any) => {
  handleUid.value = record.uid
  visible.value = true;
}
const handleOP = (valueList: string[]) => {
  for (let index = 0; index < valueList.length; index++) {
    const element = valueList[index];
    var i = element.search("@") + 1
    send(insertStr(element, i))
  }

}
function send(cmd: string) {
  const send_msg = {
    type: 'CMD',
    data: cmd,
  }
  const send_msg_str = JSON.stringify(send_msg)
  appStore.socketSend(send_msg_str)
}
function insertStr(soure: string, start: number, newStr = handleUid.value) {
  return soure.slice(0, start) + newStr + soure.slice(start);
}
const handleOk = () => {
  visible.value = false;
}
const handleCancel = () => {
  visible.value = false;
}
const visible = ref(false);


onMounted(() => {
  updatePlayerList()
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
