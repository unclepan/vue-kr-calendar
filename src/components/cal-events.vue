<template>
  <div class="events-wrapper">
    <slot>
      <p class="no-data">正在加载日程数据列表</p>
    </slot>
  </div>
</template>

<script>
export default {
  name: 'cal-events',
  data() {
    return {
    };
  },
};
</script>
<style lang="less" scoped>
  .events-wrapper{
    .no-data{
      color: #AEB7C0;
      font-size: 13px;
      text-align: center;
      padding: 30px 0;
    }
  }
</style>
