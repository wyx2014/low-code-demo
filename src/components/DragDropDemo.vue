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
          <VantButton
            block
            plain
            type="primary"
          >
            {{ component }}
          </VantButton>
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
          :class="['canvas-item', { selected: selectedItem === item }]"
          @click="selectItem(item)"
        >
          <component 
            :is="item.component" 
            v-bind="item.props"
          />
          <div class="item-actions" v-if="selectedItem === item">
            <div class="action-btn" @click.stop="duplicateItem(item)">
              <vant-icon name="notes-o" />
            </div>
            <div class="action-btn" @click.stop="deleteItem(item)">
              <vant-icon name="delete-o"/>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Right Panel: Field Properties -->
    <div class="properties-panel">
      <template v-if="selectedItem">
        <VantField
          v-if="selectedItem.component === 'VantButton'"
          v-model="selectedItem.props.text"
          label="Button Text"
          placeholder="Enter button text"
        />
        <VantField
          v-if="selectedItem.component === 'VantButton'"
          v-model="selectedItem.props.type"
          label="Button Type"
          placeholder="primary"
        />
        <VantField
          v-if="selectedItem.component === 'VantButton'"
          v-model="selectedItem.props.block"
          label="Block"
          type="checkbox"
        />
        <VantField
          v-if="selectedItem.component === 'VantButton'"
          v-model="selectedItem.props.plain"
          label="Plain"
          type="checkbox"
        />
      </template>
      <div v-else class="placeholder">
        Select a component to edit properties
      </div>
    </div>
  </div>
</template>

<script>
import * as Vant from 'vant';

export default {
  components: {
    VantButton: Vant.Button,
    VantCell: Vant.Cell,
    VantIcon: Vant.Icon,
    VantImage: Vant.Image,
    VantPopup: Vant.Popup,
    VantToast: Vant.Toast,
    VantCalendar: Vant.Calendar,
    VantCascader: Vant.Cascader,
    VantCheckbox: Vant.Checkbox,
    VantDatetimePicker: Vant.DatetimePicker,
    VantField: Vant.Field,
    VantForm: Vant.Form,
    VantNumberKeyboard: Vant.NumberKeyboard,
    VantPasswordInput: Vant.PasswordInput,
    VantPicker: Vant.Picker,
    VantRadio: Vant.Radio,
    VantRate: Vant.Rate,
    VantSearch: Vant.Search,
    VantSlider: Vant.Slider,
    VantStepper: Vant.Stepper,
    VantSwitch: Vant.Switch,
    VantSwitchCell: Vant.SwitchCell,
    VantUploader: Vant.Uploader,
    VantActionSheet: Vant.ActionSheet,
    VantDialog: Vant.Dialog,
    VantDropdownMenu: Vant.DropdownMenu,
    VantLoading: Vant.Loading,
    VantNotify: Vant.Notify,
    VantOverlay: Vant.Overlay,
    VantPullRefresh: Vant.PullRefresh,
    VantShareSheet: Vant.ShareSheet,
    VantSwipeCell: Vant.SwipeCell,
    VantBadge: Vant.Badge,
    VantCircle: Vant.Circle,
    VantCollapse: Vant.Collapse,
    VantCountDown: Vant.CountDown,
    VantDivider: Vant.Divider,
    VantEmpty: Vant.Empty,
    VantImagePreview: Vant.ImagePreview,
    VantLazyload: Vant.Lazyload,
    VantList: Vant.List,
    VantNoticeBar: Vant.NoticeBar,
    VantPopover: Vant.Popover,
    VantProgress: Vant.Progress,
    VantSkeleton: Vant.Skeleton,
    VantSteps: Vant.Steps,
    VantSticky: Vant.Sticky,
    VantSwipe: Vant.Swipe,
    VantTag: Vant.Tag,
    VantGrid: Vant.Grid,
    VantIndexBar: Vant.IndexBar,
    VantNavBar: Vant.NavBar,
    VantPagination: Vant.Pagination,
    VantSidebar: Vant.Sidebar,
    VantTab: Vant.Tab,
    VantTabbar: Vant.Tabbar,
    VantTreeSelect: Vant.TreeSelect,
    VantAddressEdit: Vant.AddressEdit,
    VantAddressList: Vant.AddressList,
    VantArea: Vant.Area,
    VantCard: Vant.Card,
    VantContactCard: Vant.ContactCard,
    VantContactEdit: Vant.ContactEdit,
    VantContactList: Vant.ContactList,
    VantCoupon: Vant.Coupon,
    VantGoodsAction: Vant.GoodsAction,
    VantSubmitBar: Vant.SubmitBar,
    VantSku: Vant.Sku
  },
  data() {
    return {
      componentGroups: [
        {
          name: 'Basic Components',
          components: [
            'VantButton', 'VantCell', 'VantIcon', 'VantImage', 'VantPopup', 'VantToast'
          ]
        },
        {
          name: 'Form Components',
          components: [
            'VantCalendar', 'VantCascader', 'VantCheckbox', 'VantDatetimePicker', 'VantField',
            'VantForm', 'VantNumberKeyboard', 'VantPasswordInput', 'VantPicker', 'VantRadio',
            'VantRate', 'VantSearch', 'VantSlider', 'VantStepper', 'VantSwitch', 'VantSwitchCell',
            'VantUploader'
          ]
        },
        {
          name: 'Action Components',
          components: [
            'VantActionSheet', 'VantDialog', 'VantDropdownMenu', 'VantLoading', 'VantNotify',
            'VantOverlay', 'VantPullRefresh', 'VantShareSheet', 'VantSwipeCell'
          ]
        },
        {
          name: 'Display Components',
          components: [
            'VantBadge', 'VantCircle', 'VantCollapse', 'VantCountDown', 'VantDivider', 'VantEmpty',
            'VantImagePreview', 'VantLazyload', 'VantList', 'VantNoticeBar', 'VantPopover',
            'VantProgress', 'VantSkeleton', 'VantSteps', 'VantSticky', 'VantSwipe', 'VantTag'
          ]
        },
        {
          name: 'Navigation Components',
          components: [
            'VantGrid', 'VantIndexBar', 'VantNavBar', 'VantPagination', 'VantSidebar', 'VantTab',
            'VantTabbar', 'VantTreeSelect'
          ]
        },
        {
          name: 'Business Components',
          components: [
            'VantAddressEdit', 'VantAddressList', 'VantArea', 'VantCard', 'VantContactCard',
            'VantContactEdit', 'VantContactList', 'VantCoupon', 'VantGoodsAction',
            'VantSubmitBar', 'VantSku'
          ]
        }
      ],
      canvasItems: [],
      draggedComponent: null,
      selectedItem: null
    };
  },
  methods: {
    onDragStart(event, component) {
      console.log('Drag start event triggered');
      this.draggedComponent = component;
      console.log('Dragging component:', component);
      event.dataTransfer.setData('text/plain', component);
      console.log('Data set:', component);
    },
    onDrop(event) {
      event.preventDefault();
      if (this.draggedComponent) {
        console.log('Dropping component:', this.draggedComponent);
        const newItem = {
          component: this.draggedComponent,
          props: this.getDefaultProps(this.draggedComponent)
        };
        this.canvasItems.push(newItem);
        this.draggedComponent = null;
        this.selectItem(newItem);
        console.log('Canvas items:', this.canvasItems);
      }
    },
    selectItem(item) {
      this.selectedItem = item;
    },
    deleteItem(item) {
      const index = this.canvasItems.indexOf(item);
      if (index > -1) {
        this.canvasItems.splice(index, 1);
        // Auto select previous item if exists
        if (this.canvasItems.length > 0) {
          const newIndex = Math.max(0, index - 1);
          this.selectItem(this.canvasItems[newIndex]);
        } else {
          this.selectedItem = null;
        }
      }
    },
    duplicateItem(item) {
      const newItem = JSON.parse(JSON.stringify(item));
      this.canvasItems.push(newItem);
      this.selectItem(newItem);
    },
    getDefaultProps(component) {
      console.log('Getting props for:', component);
      // Return default props for different components
      const defaultProps = {
        VantButton: { type: 'primary', text: 'Button' },
        VantCell: { title: 'Cell', value: 'Content' },
        VantIcon: { name: 'success' },
        VantField: { label: 'Field', placeholder: 'Enter text' },
        VantSwitch: { checked: false },
        VantCheckbox: { checked: false },
        VantRadio: { checked: false },
        VantSlider: { value: 50 },
        VantRate: { value: 3 },
        VantStepper: { value: 1 },
        VantUploader: { fileList: [] },
        VantPicker: { columns: ['Option 1', 'Option 2'] },
        VantDatetimePicker: { type: 'datetime' },
        VantCalendar: { type: 'single' },
        VantCascader: { options: [] },
        VantNumberKeyboard: { show: true },
        VantPasswordInput: { value: '' },
        VantSearch: { value: '', placeholder: 'Search' },
        VantSwitchCell: { title: 'Switch', checked: false },
        VantForm: { model: {} },
        VantPopup: { show: false },
        VantToast: { message: 'Toast' },
        VantDialog: { show: false },
        VantActionSheet: { show: false },
        VantDropdownMenu: { options: [] },
        VantLoading: { show: false },
        VantNotify: { show: false },
        VantOverlay: { show: false },
        VantPullRefresh: { refreshing: false },
        VantShareSheet: { show: false },
        VantSwipeCell: { leftWidth: 100, rightWidth: 100 },
        VantBadge: { content: '1' },
        VantCircle: { value: 0 },
        VantCollapse: { value: [] },
        VantCountDown: { time: 60000 },
        VantDivider: { dashed: false },
        VantEmpty: { description: 'No Data' },
        VantImagePreview: { show: false },
        VantLazyload: { loading: 'loading.png' },
        VantList: { loading: false, finished: false },
        VantNoticeBar: { text: 'Notice' },
        VantPopover: { show: false },
        VantProgress: { percentage: 0 },
        VantSkeleton: { row: 3 },
        VantSteps: { active: 0 },
        VantSticky: { offsetTop: 0 },
        VantSwipe: { autoplay: 3000 },
        VantTag: { type: 'primary', text: 'Tag' },
        VantGrid: { columnNum: 4 },
        VantIndexBar: { indexList: ['A', 'B'] },
        VantNavBar: { title: 'Title' },
        VantPagination: { totalItems: 0 },
        VantSidebar: { activeKey: 0 },
        VantTab: { active: 0 },
        VantTabbar: { active: 0 },
        VantTreeSelect: { mainActiveIndex: 0 },
        VantAddressEdit: { show: false },
        VantAddressList: { list: [] },
        VantArea: { areaList: {} },
        VantCard: { price: '0.00' },
        VantContactCard: { name: 'Name', tel: '1234567890' },
        VantContactEdit: { contact: {} },
        VantContactList: { list: [] },
        VantCoupon: { coupons: [] },
        VantGoodsAction: { actions: [] },
        VantSubmitBar: { price: '0.00' },
        VantSku: { show: false }
      };
      
      return defaultProps[component] || {};
    },
    mounted() {
      console.log('Available components:', Object.keys(this.$options.components));
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
  margin-bottom: 8px;
  cursor: move;
  text-transform: capitalize;
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
  padding: 4px;
  border-radius: 4px;
  transition: all 0.2s ease;
  position: relative;
}

.canvas-item:hover {
  border-color: #ddd;
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

.action-btn:hover {
  background: #f5f5f5;
}

.action-btn:first-child {
  background: #ecf5ff;
  color: #409eff;
  border: 1px solid #b3d8ff;
}

.action-btn:first-child:hover {
  background: #d9ecff;
}

.action-btn:last-child {
  background: #fef0f0;
  color: #f56c6c;
  border: 1px solid #fbc4c4;
}

.action-btn:last-child:hover {
  background: #fde2e2;
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
