<template>
  <div class="canvas" @dragover.prevent @drop="onDrop">
    <div
      v-for="(item, index) in canvasItems"
      :key="index"
      :class="['canvas-item', { selected: selectedItem === item }]"
      :data-component="item.component"
      draggable="true"
      @dragstart="onDragStartCanvasItem($event, index)"
      @dragend="onDragEndCanvasItem"
      @dragover.prevent="onDragOverCanvasItem($event, index)"
      @dragleave="onDragLeaveCanvasItem"
      @click.stop="selectItem(item, $event)"
    >
      <component :is="item.component" v-bind="item.props">
        <template v-if="(item.component === 'van-col' || item.component === 'van-row')">
          <div 
            class="nested-canvas"
            @dragover.prevent="onNestedDragOver($event)"
            @dragleave.prevent="onNestedDragLeave($event)"
            @drop="onNestedDrop($event, item)"
            :class="{ 'drag-over': isDragOver }"
            :style="{
              display: item.component === 'van-row' ? 'flex' : 'block',
              flexDirection: item.component === 'van-row' ? 'row' : 'column',
              gap: item.component === 'van-row' ? (item.props.gutter) + 'px' : '0',
              alignItems: item.component === 'van-row' ? (item.props.align || 'top') : 'stretch',
              justifyContent: item.component === 'van-row' ? (item.props.justify || 'start') : 'flex-start'
            }"
          >
            <component
              v-for="(child, i) in item.props.children"
              :key="i"
              :is="child.component"
              :data-component="child.component"
              v-bind="child.props"
              @click.stop="selectItem(child, $event)"
            />
          </div>
        </template>
      </component>
      <div class="item-actions" v-if="selectedItem === item">
        <div class="action-btn" @click.stop="duplicateItem(item)">
          <van-icon name="notes-o" />
        </div>
        <div class="action-btn" @click.stop="deleteItem(item)">
          <van-icon name="delete-o" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DragCanvas',
  props: ['canvasItems', 'selectedItem'],
  data() {
    return {
      isDragOver: false,
    };
  },
  methods: {
    onDragStartCanvasItem(event, index) {
      this.$emit('drag-start-canvas-item', index);
    },
    onDragEndCanvasItem() {
      this.$emit('drag-end-canvas-item');
    },
    onDragOverCanvasItem(event, index) {
      event.preventDefault();
      this.isDragOver = true;
      this.$emit('drag-over-canvas-item', index);
    },
    onDragLeaveCanvasItem() {
      this.isDragOver = false;
      this.$emit('drag-leave-canvas-item');
    },
    onDrop(event) {
      event.preventDefault();
      this.isDragOver = false;
      this.$emit('drop', event);
    },
    onNestedDragOver(event) {
      event.preventDefault();
      this.isDragOver = true;
    },
    onNestedDragLeave() {
      this.isDragOver = false;
    },
    onNestedDrop(event, parentItem) {
      event.preventDefault();
      this.isDragOver = false;
      const newItem = {
        component: this.$parent.draggedComponent,
        props: this.$parent.getDefaultProps(this.$parent.draggedComponent),
      };
      if (!parentItem.props.children) {
        this.$set(parentItem.props, 'children', []);
      }
      parentItem.props.children.push(newItem);
      this.$parent.draggedComponent = null;
    },
    selectItem(item, event) {
      if (event) {
        event.stopPropagation();
      }
      this.$emit('select-item', item);
    },
    duplicateItem(item) {
      this.$emit('duplicate-item', item);
    },
    deleteItem(item) {
      this.$emit('delete-item', item);
    },
  },
};
</script>

<style scoped>
.canvas {
  height: calc(100vh - 100px);
  width: calc((100vh - 100px) * 0.461); /* 375/812 = 0.461 */
  max-width: 375px;
  max-height: 812px;
  background-color: white;
  border-radius: 40px;
  box-shadow: 0 0 0 11px #1f1f1f, 0 0 0 13px #191919, 0 0 0 20px #111;
  padding: 20px 20px 40px; /* Added bottom padding for home indicator */
  position: relative;
  overflow-y: auto;
  margin: auto;
  transform-origin: center;
}

.canvas-item {
  cursor: move;
  border: 1px solid transparent;
  border-radius: 4px;
  transition: all 0.2s ease;
  position: relative;
}

.canvas-item.selected {
  border: 2px solid #1989fa;
  background-color: rgba(25, 137, 250, 0.1);
  box-shadow: 0 0 8px rgba(25, 137, 250, 0.2);
}

.item-actions {
  position: absolute;
  right: 4px;
  bottom: -12px;
  display: flex;
  gap: 4px;
}

.action-btn {
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s ease;
  padding: 3px;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-btn:first-child {
  background: #ecf5ff;
  color: #409eff;
  border: 1px solid #b3d8ff;
}

.action-btn:last-child {
  background: #fef0f0;
  color: #f56c6c;
  border: 1px solid #fbc4c4;
}

.nested-canvas {
  width: 100%;
  min-height: 40px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 4px;
  border: 2px dashed #ddd;
  transition: all 0.2s ease;
  position: relative;
  display: flex;
  flex-direction: column;
}

.nested-canvas.drag-over {
  border-color: #409eff;
  background: rgba(64, 158, 255, 0.1);
}
</style> 