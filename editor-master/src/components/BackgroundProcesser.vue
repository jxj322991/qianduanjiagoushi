<template>
  <div class="background-processer">
    <styled-uploader v-if="!value" @file-uploaded="handleFileUploaded"></styled-uploader>
    <image-processer
      :value="value" @change="updateUrl"
      v-else :ratio="ratio" :showDelete="true"
      @uploaded="triggerUploaded"
    >
    </image-processer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { message } from 'ant-design-vue'
import ImageProcesser from './ImageProcess.vue'
import { commonUploadCheck, UploadImgProps } from '../helper'
import StyledUploader from './StyledUploader.vue'

export default defineComponent({
  props: {
    value: {
      type: String,
      required: true
    },
    ratio: {
      type: Number
    }
  },
  components: {
    ImageProcesser,
    StyledUploader
  },
  emits: ['change', 'uploaded'],
  setup (props, context) {
    const handleFileUploaded = (uploadedData: UploadImgProps) => {
      message.success('上传成功')
      context.emit('change', uploadedData.data.urls[0])
      context.emit('uploaded', uploadedData)
    }
    const updateUrl = (value: string) => {
      context.emit('change', value)
    }
    const triggerUploaded = (uploadedData: UploadImgProps) => {
      context.emit('uploaded', uploadedData)
    }
    return {
      handleFileUploaded,
      commonUploadCheck,
      updateUrl,
      triggerUploaded
    }
  }
})
</script>

<style>
.delete-uploaded {
  margin-top:10px;
  display: block;
}
/* .background-processer .uploader-container {
  height: 150px;
} */
</style>
