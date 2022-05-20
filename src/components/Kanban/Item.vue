<template>
  <a-card
    style="border-radius: var(--borderRadius)"
    size="small"
    :bordered="false"
    class="mb-4 kanban-card"
  >
    <span>{{ item.name }}</span>
  </a-card>
</template>

<script>
export default {
  props: {
    item: [Object],
  },
};
</script>

<style lang="scss" scoped>
.kanban-card {
  min-width: 270px;
  margin-bottom: 10px;
  cursor: move;
  -moz-user-select: none;
  -khtml-user-select: none;
  user-select: none;
  .priority {
    display: block;
    min-width: 14px;
    min-height: 14px;
    img {
      width: 14px;
      height: 14px;
    }
  }
  .act_btn {
    margin-right: -7px;
  }
  .card_name {
    cursor: pointer;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}
</style>
