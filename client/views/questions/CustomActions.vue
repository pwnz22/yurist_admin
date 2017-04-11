<template>
  <div>
    <slot name="actions" :row-data="rowData" :row-index="rowIndex"></slot>
  </div>
</template>

<script>
  export default {
    props: {
      rowData: {
        type: Object,
        required: true
      },
      rowIndex: {
        type: Number
      }
    }
  }
</script>

<style lang="scss">
  .custom-actions {
    display: flex;

    .button {
      margin-right: 5px;

      &:last-child {
        margin-right: 0;
      }
    }
  }
</style>
