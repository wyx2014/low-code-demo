<template>
  <div class="drag-drop-demo">
    <ComponentsPanel
      :componentGroups="componentGroups"
      @drag-start="onDragStart"
    />
    <div class="canvas-container">
      <div class="canvas-header">
        <button @click="exportGeneratedCode">生成代码</button>
      </div>
      <DragCanvas
        :canvasItems="canvasItems"
        :selectedItem="selectedItem"
        @drag-start-canvas-item="onDragStartCanvasItem"
        @drag-end-canvas-item="onDragEndCanvasItem"
        @drag-over-canvas-item="onDragOverCanvasItem"
        @drag-leave-canvas-item="onDragLeaveCanvasItem"
        @drop="onDrop"
        @select-item="selectItem"
        @duplicate-item="duplicateItem"
        @delete-item="deleteItem"
      />
    </div>
    <PropertiesPanel :selectedItem="selectedItem" @update-prop="updateSelectedItemProp" />
  </div>
</template>

<script>
import ComponentsPanel from './ComponentsPanel.vue';
import DragCanvas from './Canvas.vue';
import PropertiesPanel from './PropertiesPanel.vue';

export default {
  components: {
    ComponentsPanel,
    DragCanvas,
    PropertiesPanel,
  },
  data() {
    return {
      componentGroups: [
        {
          name: "Layout Components",
          components: [
            "van-col",
            "van-row",
          ],
        },
        {
          name: "Basic Components",
          components: [
            "van-button",
            "van-cell",
            "van-icon",
            "van-image",
            "van-popup",
            "van-toast",
          ],
        },
        {
          name: "Form Components",
          components: [
            "van-calendar",
            "van-cascader",
            "van-checkbox",
            "van-datetime-picker",
            "van-field",
            "van-form",
            "van-number-keyboard",
            "van-password-input",
            "van-picker",
            "van-radio",
            "van-rate",
            "van-search",
            "van-slider",
            "van-stepper",
            "van-switch",
            "van-switch-cell",
            "van-uploader",
          ],
        },
        {
          name: "Action Components",
          components: [
            "van-action-sheet",
            "van-dialog",
            "van-dropdown-menu",
            "van-loading",
            "van-notify",
            "van-overlay",
            "van-pull-refresh",
            "van-share-sheet",
            "van-swipe-cell",
          ],
        },
        {
          name: "Display Components",
          components: [
            "van-badge",
            "van-circle",
            "van-collapse",
            "van-count-down",
            "van-divider",
            "van-empty",
            "van-image-preview",
            "van-lazyload",
            "van-list",
            "van-notice-bar",
            "van-popover",
            "van-progress",
            "van-skeleton",
            "van-steps",
            "van-sticky",
            "van-swipe",
            "van-tag",
          ],
        },
        {
          name: "Navigation Components",
          components: [
            "van-grid",
            "van-index-bar",
            "van-nav-bar",
            "van-pagination",
            "van-sidebar",
            "van-tab",
            "van-tabbar",
            "van-tree-select",
          ],
        },
        {
          name: "Business Components",
          components: [
            "van-address-edit",
            "van-address-list",
            "van-area",
            "van-card",
            "van-contact-card",
            "van-contact-edit",
            "van-contact-list",
            "van-coupon",
            "van-goods-action",
            "van-submit-bar",
            "van-sku",
          ],
        },
      ],
      canvasItems: [],
      draggedComponent: null,
      selectedItem: null,
      isDragOver: false,
      draggedIndex: null,
      dropIndex: null,
    };
  },
  methods: {
    onDragStart(component) {
      this.draggedComponent = component;
    },
    onDragStartCanvasItem(index) {
      this.draggedIndex = index;
    },
    onDragEndCanvasItem() {
      this.draggedIndex = null;
    },
    onDragOverCanvasItem(index) {
      this.dropIndex = index;
    },
    onDragLeaveCanvasItem() {
      // Handle drag leave
    },
    onDrop() {
      if (this.draggedComponent) {
        const newItem = {
          component: this.draggedComponent,
          props: this.getDefaultProps(this.draggedComponent),
        };
        this.canvasItems.push(newItem);
        this.draggedComponent = null;
        this.selectItem(newItem);
      } else if (this.draggedIndex !== null) {
        if (this.dropIndex !== null && this.dropIndex !== this.draggedIndex) {
          const movedItem = this.canvasItems.splice(this.draggedIndex, 1)[0];
          this.canvasItems.splice(this.dropIndex, 0, movedItem);
        }
      }
      this.dropIndex = null;
    },
    selectItem(item) {
      this.selectedItem = item;
    },
    duplicateItem(item) {
      const newItem = JSON.parse(JSON.stringify(item));
      this.canvasItems.push(newItem);
      this.selectItem(newItem);
    },
    deleteItem(item) {
      const index = this.canvasItems.indexOf(item);
      if (index > -1) {
        this.canvasItems.splice(index, 1);
        if (this.canvasItems.length > 0) {
          const newIndex = Math.max(0, index - 1);
          this.selectItem(this.canvasItems[newIndex]);
        } else {
          this.selectedItem = null;
        }
      }
    },
    updateSelectedItemProp({ propName, value }) {
      if (this.selectedItem && this.selectedItem.props) {
        this.$set(this.selectedItem.props, propName, value);
      }
    },
    getDefaultProps(component) {
      const defaultProps = {
        'van-col': { 
          span: 12,
          offset: 0,
          tag: 'div', 
          children: []
        },
        'van-row': { 
          gutter: 0,
          tag: 'div',
          justify: 'start',
          align: 'top',
          children: []
        },
        'van-button': { type: "primary", text: "Button" },
        'van-cell': { title: "Cell", value: "Content" },
        'van-icon': { name: "success" },
        'van-field': { label: "Field", placeholder: "Enter text" },
        'van-switch': { checked: false },
        'van-checkbox': { checked: false },
        'van-radio': { checked: false },
        'van-slider': { value: 50 },
        'van-rate': { value: 3 },
        'van-stepper': { value: 1 },
        'van-uploader': { fileList: [] },
        'van-picker': { columns: ["Option 1", "Option 2"] },
        'van-datetime-picker': { type: "datetime" },
        'van-calendar': { type: "single" },
        'van-cascader': { options: [] },
        'van-number-keyboard': { show: true },
        'van-password-input': { value: "" },
        'van-search': { value: "", placeholder: "Search" },
        'van-switch-cell': { title: "Switch", checked: false },
        'van-form': { model: {} },
        'van-popup': { show: false },
        'van-toast': { message: "Toast" },
        'van-dialog': { show: false },
        'van-action-sheet': { show: false },
        'van-dropdown-menu': { options: [] },
        'van-loading': { show: false },
        'van-notify': { show: false },
        'van-overlay': { show: false },
        'van-pull-refresh': { refreshing: false },
        'van-share-sheet': { show: false },
        'van-swipe-cell': { leftWidth: 100, rightWidth: 100 },
        'van-badge': { content: "1" },
        'van-circle': { value: 0 },
        'van-collapse': { value: [] },
        'van-count-down': { time: 60000 },
        'van-divider': { dashed: false },
        'van-empty': { description: "No Data" },
        'van-image-preview': { show: false },
        'van-lazyload': { loading: "loading.png" },
        'van-list': { loading: false, finished: false },
        'van-notice-bar': { text: "Notice" },
        'van-popover': { show: false },
        'van-progress': { percentage: 0 },
        'van-skeleton': { row: 3 },
        'van-steps': { active: 0 },
        'van-sticky': { offsetTop: 0 },
        'van-swipe': { autoplay: 3000 },
        'van-tag': { type: "primary", text: "Tag" },
        'van-grid': { columnNum: 4 },
        'van-index-bar': { indexList: ["A", "B"] },
        'van-nav-bar': { title: "Title" },
        'van-pagination': { totalItems: 0 },
        'van-sidebar': { activeKey: 0 },
        'van-tab': { active: 0 },
        'van-tabbar': { active: 0 },
        'van-tree-select': { mainActiveIndex: 0 },
        'van-address-edit': { show: false },
        'van-address-list': { list: [] },
        'van-area': { areaList: {} },
        'van-card': { price: "0.00" },
        'van-contact-card': { name: "Name", tel: "1234567890" },
        'van-contact-edit': { contact: {} },
        'van-contact-list': { list: [] },
        'van-coupon': { coupons: [] },
        'van-goods-action': { actions: [] },
        'van-submit-bar': { price: "0.00" },
        'van-sku': { show: false },
      };
      return defaultProps[component] || {};
    },
    generateVueCode() {
      const generateComponentCode = (item) => {
        const { component, props } = item;
        const propString = Object.entries(props)
          .map(([key, value]) => {
            if (typeof value === 'boolean') {
              return value ? key : '';
            }
            return `${key}="${value}"`;
          })
          .filter(Boolean)
          .join(' ');

        let childrenCode = '';
        if (props.children && props.children.length > 0) {
          childrenCode = props.children.map(generateComponentCode).join('\n');
        }

        return `<${component} ${propString}>${childrenCode}</${component}>`;
      };

      const templateCode = this.canvasItems.map(generateComponentCode).join('\n');


      return `<template>\n<div>\n${templateCode}\n</div>\n</template>\n`;
    },
    exportGeneratedCode() {
      const code = this.generateVueCode();
      const blob = new Blob([code], { type: 'text/plain;charset=utf-8' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'generated-component.vue';
      a.click();
      URL.revokeObjectURL(url);
    },
  },
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

.canvas-container {
  flex: 5;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f5f5f5;
  padding: 16px;
  overflow-y: auto;
  position: relative;
}

.canvas-header {
  position: absolute;
  top: 0;
  right: 0;
  padding: 8px;
}

.properties-panel {
  flex: 2;
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
