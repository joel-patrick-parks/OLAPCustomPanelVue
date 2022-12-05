<template>
<div>
  <div class="flextable">
    <wj-pivot-panel :items-source="ng"></wj-pivot-panel>
    <wj-pivot-grid :items-source="ng"></wj-pivot-grid>
  </div>
</div>
</template>

<script>
import '@grapecity/wijmo.styles/wijmo.css';

import { PivotEngine } from '@grapecity/wijmo.olap';
import * as wjOlap from '@grapecity/wijmo.olap';
import * as wjcOlap from '@grapecity/wijmo.vue2.olap';

export default {
  name: 'App',
  components: {
    'wj-pivot-panel': wjcOlap.WjPivotPanel,
    'wj-pivot-grid': wjcOlap.WjPivotGrid
  },
  setup() {
    wjOlap.PivotPanel.controlTemplate = `<div class="root">  
      <div class="field-list-label">  
        <label wj-part="g-flds"></label>  
      </div>  
      <div class="field-list pad">  
        <div wj-part="d-fields"></div>  
      </div>  
      <div class="drag-areas-label">  
        <label wj-part="g-drag"></label>  
      </div>  
      <div class="filter-list pad">  
        <label>  
          <span class="wj-glyph wj-glyph-filter"></span>  
          <span wj-part="g-flt"></span>  
        </label>  
        <div wj-part="d-filters"></div>  
      </div>  
      <div class="column-list pad bdr-left">  
        <label>  
          <span class="wj-glyph">⫴</span>  
          <span wj-part="g-cols"></span>  
        </label>  
        <div wj-part="d-cols"></div>  
      </div>  
      <div class="row-list pad bdr-top">  
        <label>  
          <span class="wj-glyph">≡</span>  
          <span wj-part="g-rows"></span>  
        </label>  
        <div wj-part="d-rows"></div>  
      </div>  
      <div class="values-list pad bdr-left bdr-top">  
        <label>  
          <span class="wj-glyph">Σ</span>  
          <span wj-part="g-vals"></span>  
        </label>  
        <div wj-part="d-vals"></div>  
      </div>  
      <div wj-part="d-prog" class="progress-bar"></div>  
      <div class="control-area">  
        <label>  
          <input wj-part="chk-defer" type="checkbox">  
          <span wj-part="g-defer">Defer Updates</span>  
        </label>  
        <button wj-part="btn-update" class="wj-btn wj-state-disabled" type="button" disabled>
          Update  
        </button>  
      </div>  
    </div>`;

    var products = [
      { product: 'Wijmo', platform: 'Web' },
      { product: 'ActiveReports', platform: 'Desktop' },
      { product: 'ActiveReportsJS', platform: 'Web' },
      { product: 'ComponentOne', platform: 'Desktop' },
      { product: 'Spread', platform: 'Desktop' },
      { product: 'SpreadJS', platform: 'Web' },
      { product: 'GCDocs', platform: 'Desktop' }
    ];

    var agents = [
      { agent: 'Ashlyn Dunlop', region: 'East' },
      { agent: 'Keith Vang', region: 'East' },
      { agent: 'Bobbi Rodrigues', region: 'West' },
      { agent: 'Charli Medina', region: 'West' },
      { agent: 'Kaitlin Salt', region: 'West' },
    ];

    var ng = new PivotEngine({
      itemsSource: getOrdersList(1000),
      fields:[
        { binding: 'date', header: 'Quarter', format: '"Q"q yyyy' },
        { binding: 'date', header: 'Month', format: 'MMMM' },
        { binding: 'agent', header: 'Agent' },
        { binding: 'region', header: 'Region' },
        { binding: 'platform', header: 'Platform' },
        { binding: 'product', header: 'Product' },
        { binding: 'sales', header: 'Sales', format: 'c2' },
        { binding: 'downloads', header: 'Downloads', format: 'n0' },
        { binding: 'revenue', header: 'Revenue', format: 'c2' },
      ]
    });

    function randomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    }

    function getOrdersList(count) {
      var year = new Date().getFullYear(), data = [];
      for(var i = 0; i < count; i++) {
        let productIdx = randomInt(0, 6);
        let agentIdx = randomInt(0, 4);
        data.push({
          orderId: randomInt(1, 10000),
          platform: products[productIdx].platform,
          product: products[productIdx].product,
          agent: agents[agentIdx].agent,
          region: agents[agentIdx].region,
          date: new Date(year - randomInt(0, 2), randomInt(0, 11), randomInt(0, 27)),
          sales: randomInt(10, 50),
          downloads: randomInt(10, 200),
          revenue: randomInt(500, 3500)
        });
      }
      return data;
    }

    return { ng }
  }
}
</script>

<style>
body {
    background: #141414;
}

.flextable {
    display: flex;
}

.wj-pivotgrid {
    height: 600px;
    width: 1200px;
    border: 1px solid #333;
    background: #333;
}

/* PivotPanel Styles */

.wj-pivotpanel {
    min-height: 0;
    height: 400px;
    width: 650px !important;
    margin: 0px 10px 0px 0px;
    display: block;
    background: #333;
    color: white;
    padding: 10px;
    padding-right: 20px;
}

.wj-pivotpanel .wj-flexgrid {
    min-height: 4em;
    max-height: 310px;
    background: inherit;
    color: white;
}

.wj-pivotpanel .root {
    display: -ms-grid;
    display: grid;
    grid-template-columns: repeat(4, 25%);
    height: 100%;
}

.wj-pivotpanel .field-list-label {
    -ms-grid-column: 1;
    -ms-grid-column-span: 2;
    grid-column: 1/span 2;
    -ms-grid-row: 1;
    grid-row: 1;
}

.wj-pivotpanel .field-list {
    -ms-grid-column: 1;
    -ms-grid-column-span: 2;
    grid-column: 1/span 2;
    -ms-grid-row: 2;
    -ms-grid-row-span: 2;
    grid-row: 2/span 2;
    border: 1px solid rgba(0, 0, 0, .2);
    margin-top: 5px;
    height: 322px;
}

.wj-pivotpanel .drag-areas-label {
    -ms-grid-column: 3;
    -ms-grid-column-span: 2;
    grid-column: 3/span 2;
    -ms-grid-row: 1;
    grid-row: 1;
}

.wj-pivotpanel .filter-list {
    -ms-grid-column: 3;
    grid-column: 3;
    -ms-grid-row: 2;
    grid-row: 2;
    height: 95%;
    border: 1px solid rgba(0, 0, 0, .2);
    margin: 5px;
}

.wj-pivotpanel .column-list {
    -ms-grid-column: 4;
    grid-column: 4;
    -ms-grid-row: 2;
    grid-row: 2;
    height: 95%;
    border: 1px solid rgba(0, 0, 0, .2);
    margin: 5px;
    margin-left: 10px;
}

.wj-pivotpanel .row-list {
    -ms-grid-column: 3;
    grid-column: 3;
    -ms-grid-row: 3;
    grid-row: 3;
    height: 95%;
    border: 1px solid rgba(0, 0, 0, .2);
    margin: 5px;
}

.wj-pivotpanel .values-list {
    -ms-grid-column: 4;
    grid-column: 4;
    -ms-grid-row: 3;
    grid-row: 3;
    height: 95%;
    border: 1px solid rgba(0, 0, 0, .2);
    margin: 5px;
    margin-left: 10px;
}

.wj-pivotpanel .progress-bar {
    -ms-grid-column: 1;
    -ms-grid-column-span: 4;
    grid-column: 1/span 4;
    -ms-grid-row: 4;
    grid-row: 4;
    width: 0px;
    height: 3px;
}

.wj-pivotpanel .control-area {
    -ms-grid-column: 1;
    -ms-grid-column-span: 4;
    grid-column: 1/span 4;
    -ms-grid-row: 4;
    grid-row: 4;
    display: -ms-grid;
    display: grid;
    -webkit-box-align: end;
    -ms-flex-align: end;
    align-items: end;
    -ms-grid-columns: 1fr auto;
    grid-template-columns: 1fr auto;
}

.wj-pivotpanel .control-area button {
    -ms-grid-column: 2;
    grid-column: 2;
    align-self: end;
    -ms-grid-column-align: end;
    margin-top: 3px;
    margin-right: -10px;
}

.wj-pivotpanel .pad {
    padding: 6px;
}

.wj-pivotpanel .values-list .wj-flexgrid {
    height: 118px;
}

.wj-pivotpanel .filter-list .wj-flexgrid {
    height: 118px;
}

.wj-pivotpanel .column-list .wj-flexgrid {
    height: 118px;
}

.wj-pivotpanel .row-list .wj-flexgrid {
    height: 118px;
}

.wj-control a.wj-btn, .wj-viewer .wj-control a.wj-applybutton, .wj-control button.wj-btn:not(.wj-btn-default), .wj-viewer .wj-control button.wj-applybutton:not(.wj-btn-default) {
    border: 1px solid rgb(66, 66, 66);
    border-style: solid;
    color: white;
    background-color: #3f51b5;
}

.wj-control a.wj-btn:hover, .wj-viewer .wj-control a.wj-applybutton:hover, .wj-control button.wj-btn:not(.wj-btn-default):hover, .wj-viewer .wj-control button.wj-applybutton:not(.wj-btn-default):hover {
    border: 1px solid rgb(66, 66, 66);
    border-style: solid;
    color: white;
    background-color: #5d6fd4;
}

/* PivotGrid Styles */

.wj-cell {
    background: #333;
    color: white;
}

.wj-cell.wj-header {
    background: #595959;
    color: white;
}

.wj-cell.wj-aggregate {
    background: #595959 !important;
    color: white;
}

.wj-cell.wj-align-right.wj-aggregate {
    background: #595959 !important;
}
</style>