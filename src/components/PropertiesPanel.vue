<template>
  <div class="properties-panel">
    <template v-if="selectedItem">
      <el-form :model="localProps" label-width="120px">
        <!-- 根据 selectedItem.component 动态渲染表单项 -->
        <el-form-item label="Type" v-if="selectedItem.component === 'van-button'">
          <el-select v-model="localProps.type" placeholder="Select type" @change="updateProp('type', localProps.type)">
            <el-option label="Default" value="default"></el-option>
            <el-option label="Primary" value="primary"></el-option>
            <el-option label="Info" value="info"></el-option>
            <el-option label="Warning" value="warning"></el-option>
            <el-option label="Danger" value="danger"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Size" v-if="selectedItem.component === 'van-button'">
          <el-select v-model="localProps.size" placeholder="Select size" @change="updateProp('size', localProps.size)">
            <el-option label="Normal" value="normal"></el-option>
            <el-option label="Large" value="large"></el-option>
            <el-option label="Small" value="small"></el-option>
            <el-option label="Mini" value="mini"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Text" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.text" placeholder="Button text" @input="updateProp('text', localProps.text)" />
        </el-form-item>
        <el-form-item label="Color" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.color" placeholder="Button color" @input="updateProp('color', localProps.color)" />
        </el-form-item>
        <el-form-item label="Icon" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.icon" placeholder="Icon name or URL" @input="updateProp('icon', localProps.icon)" />
        </el-form-item>
        <el-form-item label="Icon Prefix" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.iconPrefix" placeholder="Icon prefix" @input="updateProp('iconPrefix', localProps.iconPrefix)" />
        </el-form-item>
        <el-form-item label="Icon Position" v-if="selectedItem.component === 'van-button'">
          <el-select v-model="localProps.iconPosition" placeholder="Select position" @change="updateProp('iconPosition', localProps.iconPosition)">
            <el-option label="Left" value="left"></el-option>
            <el-option label="Right" value="right"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Tag" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.tag" placeholder="HTML tag" @input="updateProp('tag', localProps.tag)" />
        </el-form-item>
        <el-form-item label="Native Type" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.nativeType" placeholder="Native type" @input="updateProp('nativeType', localProps.nativeType)" />
        </el-form-item>
        <el-form-item label="Block" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.block" @change="updateProp('block', localProps.block)"></el-switch>
        </el-form-item>
        <el-form-item label="Plain" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.plain" @change="updateProp('plain', localProps.plain)"></el-switch>
        </el-form-item>
        <el-form-item label="Square" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.square" @change="updateProp('square', localProps.square)"></el-switch>
        </el-form-item>
        <el-form-item label="Round" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.round" @change="updateProp('round', localProps.round)"></el-switch>
        </el-form-item>
        <el-form-item label="Disabled" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.disabled" @change="updateProp('disabled', localProps.disabled)"></el-switch>
        </el-form-item>
        <el-form-item label="Hairline" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.hairline" @change="updateProp('hairline', localProps.hairline)"></el-switch>
        </el-form-item>
        <el-form-item label="Loading" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.loading" @change="updateProp('loading', localProps.loading)"></el-switch>
        </el-form-item>
        <el-form-item label="Loading Text" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.loadingText" placeholder="Loading text" @input="updateProp('loadingText', localProps.loadingText)" />
        </el-form-item>
        <el-form-item label="Loading Type" v-if="selectedItem.component === 'van-button'">
          <el-select v-model="localProps.loadingType" placeholder="Select type" @change="updateProp('loadingType', localProps.loadingType)">
            <el-option label="Circular" value="circular"></el-option>
            <el-option label="Spinner" value="spinner"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Loading Size" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.loadingSize" placeholder="Loading size" @input="updateProp('loadingSize', localProps.loadingSize)" />
        </el-form-item>
        <el-form-item label="URL" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.url" placeholder="URL" @input="updateProp('url', localProps.url)" />
        </el-form-item>
        <el-form-item label="To" v-if="selectedItem.component === 'van-button'">
          <el-input v-model="localProps.to" placeholder="Route object" @input="updateProp('to', localProps.to)" />
        </el-form-item>
        <el-form-item label="Replace" v-if="selectedItem.component === 'van-button'">
          <el-switch v-model="localProps.replace" @change="updateProp('replace', localProps.replace)"></el-switch>
        </el-form-item>
        <el-form-item label="Type" v-if="selectedItem.component === 'van-col'">
          <el-select v-model="localProps.type" placeholder="Select layout type" @change="updateProp('type', localProps.type)">
            <el-option label="Flex" value="flex"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Gutter" v-if="selectedItem.component === 'van-col'">
          <el-input v-model="localProps.gutter" placeholder="Gutter size" @input="updateProp('gutter', localProps.gutter)" />
        </el-form-item>
        <el-form-item label="Tag" v-if="selectedItem.component === 'van-col'">
          <el-input v-model="localProps.tag" placeholder="Custom tag" @input="updateProp('tag', localProps.tag)" />
        </el-form-item>
        <el-form-item label="Justify" v-if="selectedItem.component === 'van-col'">
          <el-select v-model="localProps.justify" placeholder="Select justify" @change="updateProp('justify', localProps.justify)">
            <el-option label="Start" value="start"></el-option>
            <el-option label="End" value="end"></el-option>
            <el-option label="Center" value="center"></el-option>
            <el-option label="Space Around" value="space-around"></el-option>
            <el-option label="Space Between" value="space-between"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Align" v-if="selectedItem.component === 'van-col'">
          <el-select v-model="localProps.align" placeholder="Select align" @change="updateProp('align', localProps.align)">
            <el-option label="Top" value="top"></el-option>
            <el-option label="Center" value="center"></el-option>
            <el-option label="Bottom" value="bottom"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Gutter" v-if="selectedItem.component === 'van-row'">
          <el-input v-model="localProps.gutter" placeholder="Gutter size" @input="updateProp('gutter', localProps.gutter)" />
        </el-form-item>
        <el-form-item label="Justify" v-if="selectedItem.component === 'van-row'">
          <el-select v-model="localProps.justify" placeholder="Select justify" @change="updateProp('justify', localProps.justify)">
            <el-option label="Start" value="start"></el-option>
            <el-option label="End" value="end"></el-option>
            <el-option label="Center" value="center"></el-option>
            <el-option label="Space Around" value="space-around"></el-option>
            <el-option label="Space Between" value="space-between"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Align" v-if="selectedItem.component === 'van-row'">
          <el-select v-model="localProps.align" placeholder="Select align" @change="updateProp('align', localProps.align)">
            <el-option label="Top" value="top"></el-option>
            <el-option label="Middle" value="middle"></el-option>
            <el-option label="Bottom" value="bottom"></el-option>
          </el-select>
        </el-form-item>
        <!-- 添加其他组件的属性表单项 -->
      </el-form>
    </template>
    <div v-else class="placeholder">
      Select a component to edit properties
    </div>
  </div>
</template>

<script>
export default {
  props: ['selectedItem'],
  computed: {
    localProps() {
      return { ...this.selectedItem.props };
    },
  },
  methods: {
    updateProp(propName, value) {
      this.$emit('update-prop', { propName, value });
    },
  },
};
</script>

<style scoped>
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