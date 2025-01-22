<template>
  <div class="components-panel">
    <div
      v-for="group in componentGroups"
      :key="group.name"
      class="component-group"
    >
      <div class="group-title">{{ group.name }}</div>
      <div
        v-for="component in group.components"
        :key="component"
        class="draggable-component"
        draggable="true"
        @dragstart="onDragStart($event, component)"
      >
        <van-button block plain type="primary">
          {{ component }}
        </van-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['componentGroups'],
  methods: {
    onDragStart(event, component) {
      this.$emit('drag-start', component);
    },
  },
};
</script>

<style scoped>
.components-panel {
  flex: 1;
  padding: 16px;
  border-right: 1px solid #eee;
  overflow-y: auto;
}

.component-group {
  margin-bottom: 24px;
}

.group-title {
  font-size: 14px;
  font-weight: 500;
  color: #666;
  margin-bottom: 8px;
  padding-bottom: 4px;
  border-bottom: 1px solid #eee;
}

.draggable-component {
  margin-bottom: 8px;
  cursor: move;
  text-transform: capitalize;
}
</style> 