<template>
  <view>
    <!-- #ifdef H5 -->
    <iframe v-if="show" v-bind="iframeProps"></iframe>
    <!-- #endif -->
    <!-- #ifdef MP-WEIXIN -->
    <web-view
      :src="iframeSrc"
      @message="handleMessage"
      @load="handleLoad"
      @error="handleError"
    ></web-view>
    <!-- #endif -->
  </view>
</template>

<script lang="ts" setup>
import { ref, reactive, nextTick, watch } from 'vue'
import { http } from '@/utils/http'
import { useToast, useMessage, useNotify, dayjs } from 'wot-design-uni'
import { useRouter } from '@/plugin/uni-mini-router'
import { randomString } from '@/common/uitls'
import { useUserStore } from '@/store/user'
import { isH5, isApp, isMp } from '@/utils/platform'

const api = {
  add: '/desform/data/add',
  edit: '/desform/data/edit',
  online: '/online/cgform/api/crazyForm',
}
const props = defineProps({
  // add = 新增，edit = 修改，detail = 只读查看
  mode: {
    type: String,
    required: true,
  },
  desformCode: {
    type: String,
    default: '',
  },
  dataId: {
    type: String,
    default: null,
  },
  alert: {
    type: Boolean,
    default: true,
  },
  height: {
    type: [String, Number],
    default: null,
  },
  pageTitle: {
    type: String,
    default: '',
  },
})
const emit = defineEmits(['dialogChange', 'success', 'error'])
const userStore = useUserStore()
const toast = useToast()
const show = ref(true)
const pageLoading = ref(true)
const loading = ref(true)
const messageId = ref(randomString('16'))
const iframeHeight = ref(0)
const latitude = ref()
const longitude = ref()
// 小程序webview
const iframeSrc = computed(() => {
  let domainURL = import.meta.env.VITE_SERVER_BASEURL
  let dataId = props.dataId == null ? 'add' : props.dataId
  // 拼接 iframe 的 src 属性
  let src = `${domainURL}/desform/${props.mode}/${props.desformCode}`
  //  - 修改和查看都需要传递 dataId 参数
  if (props.mode === 'edit' || props.mode === 'detail') {
    src += `/${dataId}`
  }
  let token = userStore.userInfo.token
  let tenantId = userStore.userInfo.tenantId
  src += `?messageId=${messageId.value}&token=${token}&tenantId=${tenantId}&platform=mini_program`
  if (latitude.value && longitude.value) {
    src += `&latitude=${latitude.value}&longitude=${longitude.value}`
  }
  src += `&innerRequest=false` // 关闭iframe的内部请求
  src += `&disableScroll=false` // 关闭iframe的内部滚动
  src += `&pageTitle=${encodeURIComponent(props.pageTitle)}`
  console.log('src======', src)
  return src
})
// H5平台
const iframeProps = computed(() => {
  let domainURL = import.meta.env.VITE_SERVER_BASEURL
  let dataId = props.dataId == null ? 'add' : props.dataId
  // 拼接 iframe 的 src 属性
  let src = `${domainURL}/desform/${props.mode}/${props.desformCode}`
  //  - 修改和查看都需要传递 dataId 参数
  if (props.mode === 'edit' || props.mode === 'detail') {
    src += `/${dataId}`
  }
  let token = userStore.userInfo.token
  let tenantId = userStore.userInfo.tenantId
  src += `?messageId=${messageId.value}&token=${token}&tenantId=${tenantId}`
  if (isApp) {
    src += `&platform=android_app`
  } else {
    src += `&platform=h5`
  }
  if (latitude.value && longitude.value) {
    src += `&latitude=${latitude.value}&longitude=${longitude.value}`
  }
  src += `&innerRequest=false` // 关闭iframe的内部请求
  src += `&disableScroll=false` // 关闭iframe的内部滚动
  console.log('src======', src)
  return {
    src,
    style: {
      width: '100%',
      height: typeof props.height === 'number' ? `${props.height}px` : props.height,
      overflow: 'hidden',
      transition: props.height ? null : 'height 0.3s',
    },
    frameborder: '0',
  }
})

const handleLoad = () => {
  uni.hideLoading()
  console.log('小程序webview 加载成功...')
}
const handleError = () => {
  console.log('小程序webview 加载失败...')
}
// 小程序中 handleMessage 方法必须是返回（iframe内调用uni.navigateBack）才会触发
const handleMessage = (res) => {
  console.log('小程序回调的信息', res)
  let data: any = {}
  if (res?.mp?.type) {
    data = data.mp
  } else if (res.type) {
    data = res
  }
  if (data.type) {
    switch (data.type) {
      // case 'close':
      //   handleClose();
      //   break;
      case 'message':
        saveAllData(data.detail.data[data.detail.data.length - 1])
        break
      case 'height-change':
        iframeHeight.value = data + 10
        pageLoading.value = false
        break
      case 'dialog-change':
        emit('dialogChange', data)
        break
    }
  }
}
// app平台 webview
const webview = () => {
  const navBarHeight = 44 // 导航栏高度
  const statusBarHeight = uni.getSystemInfoSync().statusBarHeight || 0
  let wv = plus.webview.create(
    '',
    '/hybrid/html/iframe_index.html',
    {
      top: `${statusBarHeight + navBarHeight}px`,
      background: 'transparent',
    },
    {
      height: props.height + 'upx',
      src: iframeProps.value.src,
      mid: messageId.value,
    },
  )
  wv.loadURL('/hybrid/html/iframe_index.html')
  // 获取当前webview的正确方式（Vue3兼容写法）
  const pages = getCurrentPages()
  if (pages.length === 0) {
    console.error('No current page found')
    return
  }
  const currentPage = pages[pages.length - 1]
  const currentWebview = currentPage.$getAppWebview?.() || plus.webview.currentWebview()
  if (currentWebview) {
    currentWebview.append(wv)
  } else {
    console.error('Failed to get current webview')
  }
  wv.overrideUrlLoading({ mode: 'reject' }, (e) => {
    let { data, type } = getRequest(e.url)
    console.log('data=====>>>>>', data)
    console.log('type=====>>>>>', type)
    //data = unescape(data);
    //type = unescape(type);
    data = data.replace(/"\[/g, '[').replace(/\]"/g, ']')
    switch (type) {
      case 'save':
        saveAllData(JSON.parse(data))
        break
      case 'height-change':
        iframeHeight.value = data + 10
        pageLoading.value = false
        break
      case 'dialog-change':
        emit('dialogChange', data)
        break
    }
    //plus.webview.currentWebview().close();
  })
}

const getRequest = (url): any => {
  let theRequest = new Object()
  let index = url.indexOf('?')
  console.log('theRequest', theRequest)
  if (index != -1) {
    let str = url.substring(index + 1)
    let strs = str.split('&')
    for (let i = 0; i < strs.length; i++) {
      theRequest[strs[i].split('=')[0]] = unescape(strs[i].split('=')[1])
    }
  }
  console.log('theRequest-->after', theRequest)

  return theRequest
}

const httpAction = (url, param, method) => {
  if (method === 'POST') {
    return http.post(url, param)
  } else {
    return http.put(url, param)
  }
}

const saveAllData = (params) => {
  let url = api.add,
    method = 'POST'
  let formData: any = {
    desformCode: props.desformCode,
    desformDataJson: JSON.stringify(params.json),
  }

  if (params.onlineForm) {
    formData['onlineFormCode'] = params.onlineForm
    formData['onlineFormDataId'] = params.onlineDataId
  }

  if (props.dataId && props.dataId != null) {
    url = api.edit
    method = 'PUT'
    formData['id'] = props.dataId
  }
  loading.value = true
  ;(() => {
    // 提交到数据表
    return httpAction(url, formData, method)
  })()
    .then((res: any) => {
      console.log('表单提交到数据表结果', res)
      if (res.success) {
        // 保存到表单设计器，的用户自定义url
        let {
          formConfig: { customRequestURL: curl },
        } = params
        if (curl instanceof Array && curl[0] && curl[0].url) {
          formData.dataId = res.result.dataId
          httpAction(curl[0].url, formData, method)
        }
      }
      return res
    })
    .then((res) => {
      console.log('表单设计器提交结果', res)
      if (res.success) {
        if (isMp || isApp) {
          emit('success', { res: res, dataId: res.result.dataId })
        } else {
          uni.$emit('success', { res: res, dataId: res.result.dataId })
          if (props.alert === true) {
            toast.success('保存成功')
          }
        }
      } else {
        emit('error', { res: res })
        if (props.alert === true && res.result.message) {
          toast.error(res.result.message)
        }
      }
    })
    .finally(() => {
      loading.value = false
    })
}

watch(
  () => pageLoading.value,
  (newValue, oldValue) => {
    if (pageLoading.value === false) {
      uni.hideLoading()
    }
  },
)
const init = () => {
  uni.showLoading({
    title: '表单加载中',
  })
  if (isApp) {
    webview()
  } else if (isH5) {
    window.addEventListener(
      'message',
      function (event) {
        let { messageId: mssage_id, type, data } = event.data
        if (messageId.value !== mssage_id) {
          return
        }
        switch (type) {
          case 'save':
            saveAllData(data)
            break
          case 'height-change':
            iframeHeight.value = data + 10
            pageLoading.value = false
            break
          case 'dialog-change':
            emit('dialogChange', data)
            break
        }
      },
      false,
    )
  }
  setTimeout(() => {
    if (pageLoading.value) {
      pageLoading.value = false
    }
  }, 6000)
}
init()
</script>

<style lang="scss" scoped></style>
