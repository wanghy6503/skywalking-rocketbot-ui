/**
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.
 * The ASF licenses this file to You under the Apache License, Version 2.0
 * (the "License"); you may not use this file except in compliance with
 * the License.  You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

<template>
  <div>
    <div class="rk-dashboard-bar flex-h" v-if="compType === 'service'">
      <div class="rk-dashboard-bar-reload">
        <svg class="icon lg vm cp rk-btn ghost" :style="`color:${!rocketGlobal.edit? '' :'#ffc107'}`" @click="handleSetEdit"><use :xlink:href="!rocketGlobal.edit?'#lock':'#lock-open'"></use></svg>
      </div>
      <div class="rk-dashboard-bar-reload">
        <svg class="icon lg vm cp rk-btn ghost" @click="handleOption"><use xlink:href="#retry"></use></svg>
      </div>
      <ToolBarSelect @onChoose="selectService" :title="this.$t('currentService')" :current="stateDashboard.currentService" :data="stateDashboard.services" icon="package"/>
      <ToolBarEndpointSelect @onChoose="selectEndpoint" :title="this.$t('currentEndpoint')" :current="stateDashboard.currentEndpoint" :data="stateDashboard.endpoints" icon="code"/>
      <ToolBarSelect @onChoose="selectInstance" :title="this.$t('currentInstance')" :current="stateDashboard.currentInstance" :data="stateDashboard.instances" icon="disk"/>
    </div>
    <div class="rk-dashboard-bar flex-h" v-if="compType === 'proxy'">
      <div class="rk-dashboard-bar-reload">
        <svg class="icon vm cp rk-btn ghost" @click="handleOption"><use xlink:href="#retry"></use></svg>
      </div>
      <ToolBarSelect @onChoose="selectService" title="Current Proxy" :current="stateDashboard.currentService" :data="stateDashboard.services" icon="package"/>
      <ToolBarEndpointSelect @onChoose="selectEndpoint" title="Current Endpoint" :current="stateDashboard.currentEndpoint" :data="stateDashboard.endpoints" icon="code"/>
      <ToolBarSelect @onChoose="selectInstance" title="Current Instance" :current="stateDashboard.currentInstance" :data="stateDashboard.instances" icon="disk"/>
    </div>
    <div class="rk-dashboard-bar flex-h" v-else-if="compType === 'database'">
      <div class="rk-dashboard-bar-reload">
        <svg class="icon lg vm cp rk-btn ghost" :style="`color:${!rocketGlobal.edit? '' :'#ffc107'}`" @click="handleSetEdit"><use :xlink:href="!rocketGlobal.edit?'#lock':'#lock-open'"></use></svg>
      </div>
      <div class="rk-dashboard-bar-reload">
        <svg class="icon lg vm cp rk-btn ghost" @click="handleOption"><use xlink:href="#retry"></use></svg>
      </div>
      <ToolBarSelect @onChoose="SELECT_DATABASE" :title="this.$t('currentDatabase')" :current="stateDashboard.currentDatabase" :data="stateDashboard.databases" icon="epic"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import ToolBarSelect from './tool-bar-select.vue';
import ToolBarEndpointSelect from './tool-bar-select-endpoint.vue';
import { State, Action, Mutation } from 'vuex-class';
@Component({components: {ToolBarSelect, ToolBarEndpointSelect}})
export default class ToolBar extends Vue {
  @Prop() private compType!: any;
  @Prop() private stateDashboard!: any;
  @Prop() private rocketGlobal!: any;
  @Prop() private rocketComps!: any;
  @Prop() private durationTime!: any;
  @Mutation('ADD_COMP') private ADD_COMP: any;
  @Action('SET_EDIT') private SET_EDIT: any;
  @Action('SELECT_SERVICE') private SELECT_SERVICE: any;
  @Action('SELECT_DATABASE') private SELECT_DATABASE: any;
  @Action('SELECT_ENDPOINT') private SELECT_ENDPOINT: any;
  @Action('SELECT_INSTANCE') private SELECT_INSTANCE: any;
  @Action('MIXHANDLE_GET_OPTION') private MIXHANDLE_GET_OPTION: any;
  get lastKey() {
    const current = this.rocketComps.tree[this.rocketComps.group]
    .children[this.rocketComps.current].children;
    if (!current.length) { return 0; }
    return current[current.length - 1].k;
  }
  private handleOption() {
    return this.MIXHANDLE_GET_OPTION({compType: this.compType, duration: this.durationTime});
  }
  private handleSetEdit() {
     this.SET_EDIT(this.rocketGlobal.edit ? false : true);
  }
  private selectService(i: any) {
    this.SELECT_SERVICE({service: i, duration: this.durationTime});
  }
  private selectEndpoint(i: any) {
    this.SELECT_ENDPOINT({endpoint: i, duration: this.durationTime});
  }
  private selectInstance(i: any) {
    this.SELECT_INSTANCE({instance: i, duration: this.durationTime});
  }
}
</script>

<style lang="scss" scoped>
.rk-dashboard-bar {
  flex-shrink: 0;
  color: #efefef;
  background-color: #333840;
}
.rk-dashboard-bar-reload{
  padding: 0 5px;
  border-right: 2px solid #252a2f;
}
</style>
