<template>
  <view
    :style="[
      {
        marginLeft: styles.marginLeft + 'px',
        marginRight: styles.marginRight + 'px',
        marginBottom: styles.marginBottom + 'px',
        marginTop: styles.marginTop + 'px',
        padding: styles.padding + 'px',
      },
    ]"
  >
    <mp-html class="richtext" :content="state.content"></mp-html>
  </view>
</template>
<script setup>
  import { reactive, onMounted } from 'vue';
  import sheep from '@/sheep';
  const props = defineProps({
    data: {
      type: Object,
      default: {},
    },
    styles: {
      type: Object,
      default() {},
    },
  });
  const state = reactive({
    content: '',
  });
  onMounted(async () => {
    const { code, data } = await sheep.$api.data.richtext(props.data.id);
    if (code === 1) {
      state.content = data.content;
    }
  });
</script>
