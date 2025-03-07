<route lang="json5" type="page">
{
  layout: 'default',
  style: {
    navigationBarTitleText: 'demo演示',
    navigationStyle: 'custom',
  },
}
</route>

<template>
  <PageLayout backRouteName="index" routeMethod="pushTab">
    <scroll-view scroll-y>
      <view class="box">
        <div class="content">
          <SelectUser label="用户" :required="true" v-model="user"></SelectUser>
        </div>
      </view>
      <view class="box">
        <div class="content">
          <SelectDept label="部门" :required="true" v-model="dept"></SelectDept>
        </div>
      </view>
      <view class="box">
        <div class="content">
          <view class="title mb-2">流程进度图组件</view>
          <ProgressMap title="流程历史跟踪" :dataSource="proDataSource"></ProgressMap>
        </div>
      </view>
      <view class="box" v-for="(item, index) in dataSource">
        <view class="content">
          <template v-if="index === 0">
            <view class="title">万年历组件</view>
            <uni-calendar
              :showMonth="true"
              @change="change"
              @monthChange="monthChange"
              :selected="selected"
            />
          </template>
          <template v-else>
            <view class="title">{{ item.title }}</view>
            <template v-if="['图片预览'].includes(item.title)">
              <wd-img
                :width="220"
                :height="120"
                src="https://jeecgos.oss-cn-beijing.aliyuncs.com/files/site/projectCase/mini/banner/10bdc1.jpg"
                @click="() => (imgPreview.show = true)"
              ></wd-img>
              <ImgPreview
                v-if="imgPreview.show"
                :urls="imgPreview.urls"
                @close="() => (imgPreview.show = false)"
              ></ImgPreview>
            </template>
            <wd-button v-else @click="handleSkip(item.path)">跳转页面</wd-button>
          </template>
        </view>
      </view>
      <view class="box gridBox">
        <view class="content">
          <view class="title">九宫格</view>
          <wd-grid :column="3">
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
            <wd-grid-item icon="picture" text="文字" />
          </wd-grid>
        </view>
      </view>
      <view class="box">
        <view class="content">
          <view class="title">列表</view>
          <wd-cell-group border clickable>
            <wd-cell title="组织管理" is-link icon="computer"></wd-cell>
            <wd-cell title="安全设置" is-link icon="setting"></wd-cell>
            <wd-cell title="个人设置" is-link icon="user"></wd-cell>
            <wd-cell title="退出登录" is-link icon="email"></wd-cell>
          </wd-cell-group>
        </view>
      </view>
      <view class="box">
        <view class="content">
          <view class="title">提示</view>
          <view class="flex flex-col">
            <wd-button custom-class="mb-2" @click="handleToast(0)">常规</wd-button>
            <wd-button custom-class="mb-2" @click="handleToast(1)">警告</wd-button>
            <wd-button custom-class="mb-2" @click="handleToast(2)">成功</wd-button>
            <wd-button custom-class="mb-2" @click="handleToast(3)">错误</wd-button>
            <wd-button custom-class="mb-2" @click="handleToast(4)">错误</wd-button>
            <wd-button @click="handleConfirm">确认提示</wd-button>
          </view>
        </view>
      </view>
    </scroll-view>
  </PageLayout>
</template>

<script lang="ts" setup>
//
import { ref } from 'vue'
import { useRouter } from '@/plugin/uni-mini-router'
import { useToast, useMessage, useNotify } from 'wot-design-uni'

const toast = useToast()
const user = ref('')
const dept = ref('')
const message = useMessage()
const { showNotify, closeNotify } = useNotify()

const router = useRouter()
const selected = ref([])
// 图片预览
const imgPreview = ref({
  show: false,
  urls: [
    'https://jeecgos.oss-cn-beijing.aliyuncs.com/files/site/projectCase/mini/banner/10bdc1.jpg',
  ],
})
const change = () => {}
const monthChange = () => {}

const proDataSource = [
  {
    activeStep: true,
    data: [
      { label: '流程节点：start' },
      { label: '申请人：神经蛙02' },
      { label: '申请时间：2023-12-06 16:15:14' },
      { label: '已完成' },
    ],
  },
  {
    activeStep: false,
    data: [
      { label: '流程节点：填写' },
      { label: '申请人：神经蛙01' },
      { label: '申请时间：2023-12-06 16:15:14' },
    ],
  },
  {
    activeStep: false,
    data: [
      { label: '流程节点：填写' },
      { label: '申请人：神经蛙03' },
      { label: '申请时间：2023-12-06 16:15:14' },
    ],
  },
]
const dataSource = ref([
  { title: '万年历组件' },
  { title: '树示例', path: '/pages/demo/tree' },
  { title: '通讯录', path: '/pages/demo/indexBar' },
  { title: '图片预览' },
  { title: '单选多选', path: '/pages/demo/selectPicker' },
  { title: '表单', path: '/pages/demo/form' },
])
const handleSkip = (path) => {
  router.push({ path: path })
}
const handleToast = (value) => {
  switch (value) {
    case 0:
      toast.info('常规提示信息')
      break
    case 1:
      toast.warning('提示信息')
      break
    case 2:
      toast.success('操作成功')
      break
    case 3:
      toast.error('手机验证码输入错误，请重新输入')
      break
    case 4:
      toast.show('普通提示信息')
      break
  }
}
const handleConfirm = (params) => {
  message
    .confirm({
      msg: '提示文案',
      title: '标题',
    })
    .then(() => {
      showNotify({ type: 'success', message: '点击了确认按钮' })
    })
    .catch(() => {
      showNotify({ type: 'warning', message: '点击了取消按钮' })
    })
}
</script>

<style lang="scss" scoped>
//
.mb-2 {
  margin-bottom: 10px;
}

.box {
  background-color: #fff;
  margin: 16px;
  .title {
    padding: 10px;
    padding-bottom: 0;
    font-size: 30upx;
    font-weight: bolder;
    color: #e4680a;
    margin-bottom: 20upx;
  }
  .wd-grid-item {
    border-right: 1px solid rgba(0, 0, 0, 0.1);
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    &:nth-child(1),
    &:nth-child(2),
    &:nth-child(3) {
      border-top: 1px solid rgba(0, 0, 0, 0.1);
    }
    &:nth-child(3n) {
      border-right: none;
    }
  }
  .content {
    :deep(.wd-button),
    :deep(.wd-img) {
      margin: 10px;
    }
  }
}
</style>
