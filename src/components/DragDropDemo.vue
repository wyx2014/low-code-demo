<template>
  <div class="drag-drop-demo">
    <!-- Left Panel: Vant Components -->
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
          <component :is="component" />
        </div>
      </div>
    </div>
    
    <!-- Center Panel: Canvas -->
    <div class="canvas-container">
      <div 
        class="canvas"
        @dragover.prevent
        @drop="onDrop"
      >
        <div 
          v-for="(item, index) in canvasItems" 
          :key="index"
          class="canvas-item"
          @click="selectItem(item)"
        >
          <component :is="item.component" />
        </div>
      </div>
    </div>
    
    <!-- Right Panel: Field Properties -->
    <div class="properties-panel">
      <van-field
        v-if="selectedItem"
        v-model="selectedItem.props"
        label="Properties"
        type="textarea"
        rows="4"
        autosize
      />
      <div v-else class="placeholder">
        Select a component to edit properties
      </div>
    </div>
  </div>
</template>

<script>
import { 
  Button, 
  Cell, 
  Field, 
  Switch, 
  Checkbox, 
  Radio, 
  Uploader, 
  DatetimePicker 
} from 'vant';

export default {
  components: {
    [Button.name]: Button,
    [Cell.name]: Cell,
    [Field.name]: Field,
    [Switch.name]: Switch,
    [Checkbox.name]: Checkbox,
    [Radio.name]: Radio,
    [Uploader.name]: Uploader,
    [DatetimePicker.name]: DatetimePicker
  },
  data() {
    return {
      componentGroups: [
        {
          name: '布局字段',
          components: [Button.name, Cell.name]
        },
        {
          name: '输入字段',
          components: [Field.name]
        },
        {
          name: '选择字段',
          components: [Switch.name, Checkbox.name, Radio.name]
        },
        {
          name: '上传字段',
          components: [Uploader.name]
        },
        {
          name: '日期字段',
          components: [DatetimePicker.name]
        }
      ],
      canvasItems: [],
      draggedComponent: null,
      selectedItem: null
    };
  },
  methods: {
    onDragStart(event, component) {
      this.draggedComponent = component;
      event.dataTransfer.setData('text/plain', component);
    },
    onDrop(event) {
      event.preventDefault();
      if (this.draggedComponent) {
        const newItem = {
          component: this.draggedComponent,
          props: ''
        };
        this.canvasItems.push(newItem);
        this.draggedComponent = null;
        this.selectItem(newItem);
      }
    },
    selectItem(item) {
      this.selectedItem = item;
    }
  }
};
</script>

<style scoped>
.drag-drop-demo {
  display: flex;
  height: 100vh;
}

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
  margin-bottom: 16px;
  cursor: move;
}

.canvas-container {
  flex: 4;
  padding: 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #f0f0f0;
}

.canvas-container {
  position: relative;
}

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

.canvas::before {
  content: '';
  position: absolute;
  top: -30px;
  left: 50%;
  transform: translateX(-50%);
  width: 35%;
  height: 20px;
  background: #1f1f1f;
  border-radius: 0 0 20px 20px;
  z-index: 1;
}

.canvas-container::after {
  content: '';
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    width: 15%;
    height: 10px;
    background: #1f1f1f;
    border-radius: 20px 20px 0 0;
    z-index: 2;
    box-shadow: 0 0 0 11px #1f1f1f;
}

.canvas-item {
  margin-bottom: 16px;
  cursor: pointer;
  border: 1px solid transparent;
}

.canvas-item:hover {
  border-color: #ddd;
}

.properties-panel {
  flex: 1;
  padding: 16px;
  border-left: 1px solid #eee;
  overflow-y: auto;
}

.placeholder {
  color: #999;
  text-align: center;
  margin-top: 20px;
}
</style>
