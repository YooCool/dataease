<template>
  <el-row class="view-panel">
    <plugin-com
      v-if="pluginShow"
      style="overflow:auto;border-right: 1px solid #e6e6e6;height: 100%;width: 100%;"
      class="attr-style theme-border-class"
      :component-name="view.type + '-style'"
      :obj="{view, param, chart, dimensionData, quotaData}"
    />
    <div
      v-else
      style="overflow:auto;border-right: 1px solid #e6e6e6;height: 100%;width: 100%;padding-right: 6px"
      class="attr-style theme-border-class"
    >
      <el-row>
        <span class="padding-lr">{{ $t('chart.shape_attr') }}</span>
        <el-collapse v-model="attrActiveNames" class="style-collapse">
          <el-collapse-item v-show="showPropertiesCollapse(['color-selector'])" name="color" :title="$t('chart.color')">
            <color-selector
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['color-selector']"
              @onColorChange="onColorChange($event,'color-selector')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['size-selector'])"
            name="size"
            :title="$t('chart.size')"
          >
            <size-selector
              :param="param"
              class="attr-selector"
              :property-inner="propertyInnerAll['size-selector']"
              :chart="chart"
              @onSizeChange="onSizeChange($event,'size-selector')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['size-selector-ant-v'])"
            name="size"
            :title="(chart.type && chart.type.includes('table')) ? $t('chart.table_config') : $t('chart.size')"
          >
            <size-selector-ant-v
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['size-selector-ant-v']"
              @onSizeChange="onSizeChange($event,'size-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['label-selector','label-selector-ant-v'])"
            name="label"
            :title="$t('chart.label')"
          >
            <label-selector
              v-if="showProperties('label-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['label-selector']"
              @onLabelChange="onLabelChange($event,'label-selector')"
            />
            <label-selector-ant-v
              v-else-if="showProperties('label-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['label-selector-ant-v']"
              @onLabelChange="onLabelChange($event,'label-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['tooltip-selector','tooltip-selector-ant-v'])"
            name="tooltip"
            :title="$t('chart.tooltip')"
          >
            <tooltip-selector
              v-if="showProperties('tooltip-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['tooltip-selector']"
              @onTooltipChange="onTooltipChange($event,'tooltip-selector')"
            />
            <tooltip-selector-ant-v
              v-else-if="showProperties('tooltip-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['tooltip-selector-ant-v']"
              @onTooltipChange="onTooltipChange($event,'tooltip-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['total-cfg'])"
            name="totalCfg"
            :title="$t('chart.total_cfg')"
          >
            <total-cfg
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['total-cfg']"
              @onTotalCfgChange="onTotalCfgChange($event,'total-cfg')"
            />
          </el-collapse-item>
        </el-collapse>
      </el-row>
      <el-row>
        <span class="padding-lr">{{ $t('chart.module_style') }}</span>
        <el-collapse v-model="styleActiveNames" class="style-collapse">
          <el-collapse-item
            v-show="showPropertiesCollapse(['x-axis-selector','x-axis-selector-ant-v'])"
            name="xAxis"
            :title="$t('chart.xAxis')"
          >
            <x-axis-selector
              v-if="showProperties('x-axis-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['x-axis-selector']"
              @onChangeXAxisForm="onChangeXAxisForm($event,'x-axis-selector')"
            />
            <x-axis-selector-ant-v
              v-else-if="showProperties('x-axis-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['x-axis-selector-ant-v']"
              @onChangeXAxisForm="onChangeXAxisForm($event,'x-axis-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['y-axis-selector','y-axis-selector-ant-v'])"
            name="yAxis"
            :title="view.type === 'chart-mix' ? $t('chart.yAxis_main') : $t('chart.yAxis')"
          >
            <y-axis-selector
              v-if="showProperties('y-axis-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['y-axis-selector']"
              @onChangeYAxisForm="onChangeYAxisForm($event,'y-axis-selector')"
            />
            <y-axis-selector-ant-v
              v-else-if="showProperties('y-axis-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['y-axis-selector-ant-v']"
              @onChangeYAxisForm="onChangeYAxisForm($event,'y-axis-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['y-axis-ext-selector','y-axis-ext-selector-ant-v'])"
            name="yAxisExt"
            :title="$t('chart.yAxis_ext')"
          >
            <y-axis-ext-selector
              v-if="showProperties('y-axis-ext-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['y-axis-ext-selector']"
              @onChangeYAxisForm="onChangeYAxisExtForm($event,'y-axis-ext-selector')"
            />
            <y-axis-ext-selector-ant-v
              v-else-if="showProperties('y-axis-ext-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['y-axis-ext-selector-ant-v']"
              @onChangeYAxisForm="onChangeYAxisExtForm($event,'y-axis-ext-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['split-selector','split-selector-ant-v'])"
            name="split"
            :title="$t('chart.split')"
          >
            <split-selector
              v-if="showProperties('split-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['split-selector']"
              @onChangeSplitForm="onChangeSplitForm($event,'split-selector')"
            />
            <split-selector-ant-v
              v-else-if="showProperties('split-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['split-selector-ant-v']"
              @onChangeSplitForm="onChangeSplitForm($event,'split-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['title-selector','title-selector-ant-v'])"
            name="title"
            :title="$t('chart.title')"
          >
            <title-selector
              v-if="showProperties('title-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['title-selector']"
              @onTextChange="onTextChange($event,'title-selector')"
            />
            <title-selector-ant-v
              v-else-if="showProperties('title-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['title-selector-ant-v']"
              @onTextChange="onTextChange($event,'title-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-show="showPropertiesCollapse(['legend-selector','legend-selector-ant-v'])"
            name="legend"
            :title="$t('chart.legend')"
          >
            <legend-selector
              v-if="showProperties('legend-selector')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['legend-selector']"
              @onLegendChange="onLegendChange($event,'legend-selector')"
            />
            <legend-selector-ant-v
              v-else-if="showProperties('legend-selector-ant-v')"
              :param="param"
              class="attr-selector"
              :chart="chart"
              :property-inner="propertyInnerAll['legend-selector-ant-v']"
              @onLegendChange="onLegendChange($event,'legend-selector-ant-v')"
            />
          </el-collapse-item>
          <el-collapse-item
            v-if="view.customStyle && view.customStyle.background"
            name="background"
            :title="$t('chart.background')"
          >
            <background-color-selector
              :param="param"
              class="attr-selector"
              :chart="chart"
              @onChangeBackgroundForm="onChangeBackgroundForm($event,'background-color-selector')"
            />
          </el-collapse-item>
        </el-collapse>
      </el-row>
    </div>
  </el-row>
</template>
<script>
import PluginCom from '@/views/system/plugin/PluginCom'
import ColorSelector from '@/views/chart/components/shape-attr/ColorSelector'
import SizeSelector from '@/views/chart/components/shape-attr/SizeSelector'
import SizeSelectorAntV from '@/views/chart/components/shape-attr/SizeSelectorAntV'
import LabelSelector from '@/views/chart/components/shape-attr/LabelSelector'
import LabelSelectorAntV from '@/views/chart/components/shape-attr/LabelSelectorAntV'
import TooltipSelector from '@/views/chart/components/shape-attr/TooltipSelector'
import TooltipSelectorAntV from '@/views/chart/components/shape-attr/TooltipSelectorAntV'
import TotalCfg from '@/views/chart/components/shape-attr/TotalCfg'
import XAxisSelector from '@/views/chart/components/component-style/XAxisSelector'
import XAxisSelectorAntV from '@/views/chart/components/component-style/XAxisSelectorAntV'
import YAxisSelector from '@/views/chart/components/component-style/YAxisSelector'
import YAxisSelectorAntV from '@/views/chart/components/component-style/YAxisSelectorAntV'
import YAxisExtSelector from '@/views/chart/components/component-style/YAxisExtSelector'
import YAxisExtSelectorAntV from '@/views/chart/components/component-style/YAxisExtSelectorAntV'
import TitleSelector from '@/views/chart/components/component-style/TitleSelector'
import TitleSelectorAntV from '@/views/chart/components/component-style/TitleSelectorAntV'
import LegendSelector from '@/views/chart/components/component-style/LegendSelector'
import LegendSelectorAntV from '@/views/chart/components/component-style/LegendSelectorAntV'
import BackgroundColorSelector from '@/views/chart/components/component-style/BackgroundColorSelector'
import SplitSelector from '@/views/chart/components/component-style/SplitSelector'
import SplitSelectorAntV from '@/views/chart/components/component-style/SplitSelectorAntV'
import { mapState } from 'vuex'

export default {
  name: 'ChartStyle',
  components: {
    SplitSelectorAntV,
    SplitSelector,
    BackgroundColorSelector,
    LegendSelectorAntV,
    LegendSelector,
    TitleSelectorAntV,
    TitleSelector,
    YAxisExtSelectorAntV,
    YAxisExtSelector,
    YAxisSelectorAntV,
    YAxisSelector,
    XAxisSelectorAntV,
    XAxisSelector,
    TotalCfg,
    TooltipSelectorAntV,
    TooltipSelector,
    LabelSelectorAntV,
    LabelSelector,
    SizeSelectorAntV,
    SizeSelector,
    ColorSelector,
    PluginCom
  },
  props: {
    chart: {
      type: Object,
      required: true
    },
    param: {
      type: Object,
      required: true
    },
    view: {
      type: Object,
      required: true
    },
    properties: {
      type: Array,
      required: true
    },
    dimensionData: {
      type: Array,
      required: false,
      default: function() {
        return []
      }
    },
    quotaData: {
      type: Array,
      required: false,
      default: function() {
        return []
      }
    },
    propertyInnerAll: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      attrActiveNames: [],
      styleActiveNames: []
    }
  },
  computed: {
    pluginShow() {
      return this.view.isPlugin && !this.batchOptStatus
    },
    ...mapState([
      'batchOptStatus'
    ])
  },
  created() {

  },
  methods: {
    showProperties(property) {
      return this.properties && this.properties.length && this.properties.includes(property)
    },
    showPropertiesCollapse(propertiesInfo) {
      let includeCount = 0
      // Property does not support mixed mode
      if (propertiesInfo.includes('no-mix') && this.chart.type.includes('mix')) {
        return false
      } else {
        propertiesInfo.forEach(property => {
          this.properties.includes(property) && includeCount++
        })
        return includeCount > 0
      }
    },
    calcStyle() {
      this.$emit('calcStyle')
    },
    onColorChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onColorChange', val)
    },
    onSizeChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onSizeChange', val)
    },
    onLabelChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onLabelChange', val)
    },
    onTooltipChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onTooltipChange', val)
    },
    onTotalCfgChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onTotalCfgChange', val)
    },
    onChangeXAxisForm(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onChangeXAxisForm', val)
    },
    onChangeYAxisForm(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onChangeYAxisForm', val)
    },
    onChangeYAxisExtForm(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onChangeYAxisExtForm', val)
    },
    onChangeSplitForm(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onChangeSplitForm', val)
    },
    onTextChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onTextChange', val)
    },
    onLegendChange(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onLegendChange', val)
    },
    onChangeBackgroundForm(val, propertyName) {
      val['propertyName'] = propertyName
      this.$emit('onChangeBackgroundForm', val)
    }
  }
}
</script>

<style lang='scss' scoped>
  .padding-lr {
    padding: 0 6px;
  }
  .col {
    width: 40%;
    flex: 1;
    padding: 10px;
    border: solid 1px #eee;
    border-radius: 5px;
    float: left;
  }

  .col + .col {
    margin-left: 10px;
  }

  .view-panel {
    display: flex;
    height: 100%;
    background-color: #f7f8fa;
  }

  .blackTheme .view-panel {
    background-color: var(--MainBG);
  }

  .el-form-item {
    margin-bottom: 0;
  }

  span {
    font-size: 12px;
  }

  .tab-header ::v-deep .el-tabs__header {
    border-top: solid 1px #eee;
    border-right: solid 1px #eee;
  }

  .tab-header ::v-deep .el-tabs__item {
    font-size: 12px;
    padding: 0 20px !important;
  }

  .blackTheme .tab-header ::v-deep .el-tabs__item {
    background-color: var(--MainBG);
  }

  .tab-header ::v-deep .el-tabs__nav-scroll {
    padding-left: 0 !important;
  }

  .tab-header ::v-deep .el-tabs__header {
    margin: 0 !important;
  }

  .tab-header ::v-deep .el-tabs__content {
    height: calc(100% - 40px);
  }

  .chart-icon {
    width: 20px;
    height: 20px;
  }

  .el-radio {
    margin: 5px;
  }

  .el-radio ::v-deep .el-radio__label {
    padding-left: 0;
  }

  .attr-style {
    height: calc(100vh - 56px - 60px - 40px - 40px);
  }

  .blackTheme .attr-style {
    color: var(--TextPrimary);
  }

  .attr-selector {
    width: 100%;
    height: 100%;
    margin: 6px 0;
    padding: 0 4px;
    display: flex;
    align-items: center;
    background-color: white
  }

  .blackTheme .attr-selector {

    background-color: var(--MainBG)
  }

  .dialog-css ::v-deep .el-dialog__title {
    font-size: 14px;
  }

  .dialog-css ::v-deep .el-dialog__header {
    padding: 20px 20px 0;
  }

  .dialog-css ::v-deep .el-dialog__body {
    padding: 10px 20px 20px;
  }

  .blackTheme .theme-border-class {
    color: var(--TextPrimary) !important;
    background-color: var(--ContentBG);
  }

  .blackTheme .padding-lr {
    border-color: var(--TableBorderColor) !important;
  }

  .blackTheme .theme-item-class {
    background-color: var(--MainBG) !important;
    border-color: var(--TableBorderColor) !important;
  }

  .icon-class {
    color: #6c6c6c;
  }

  .blackTheme .icon-class {
    color: #cccccc;
  }

  .radio-span ::v-deep .el-radio__label {
    margin-left: 4px;
  }

  .view-title-name {
    display: -moz-inline-box;
    display: inline-block;
    width: 130px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    margin-left: 45px;
  }

  ::v-deep .item-axis {
    width: 128px !important;
  }

  ::v-deep .el-slider__input {
    width: 80px !important;
  }

  ::v-deep .el-input-number--mini {
    width: 100px !important;
  }

  ::v-deep .el-slider__runway.show-input{
    width: 80px!important;
  }

  .no-senior {
    width: 100%;
    text-align: center;
    font-size: 12px;
    padding-top: 40px;
    overflow: auto;
    border-right: 1px solid #e6e6e6;
    height: 100%;
  }

  .form-item-slider ::v-deep .el-form-item__label{
    font-size: 12px;
    line-height: 38px;
  }
  .form-item ::v-deep .el-form-item__label{
    font-size: 12px;
  }
  </style>
