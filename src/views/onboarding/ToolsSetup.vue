<template>
  <div id="tools-setup">
    <div id="tools-init" v-if="selected === 'empty'">
      <router-link to="/" class="arrow go-back right"></router-link>
      <h4 class="title">
        ESP-IDF Tools Configuration
      </h4>
      <div class="field is-grouped is-grouped-centered">
        <div class="control">
          <button class="button" href="#" v-on:click="selectToolsSetup('auto')">
            Download ESP-IDF Tools
          </button>
        </div>
        <div class="control">
          <button
            class="button"
            href="#"
            v-on:click="selectToolsSetup('manual')"
          >
            Skip ESP-IDF Tools download
          </button>
        </div>
      </div>
    </div>
    <ToolsDownloadStep
      v-if="selected === 'auto'"
      :isPyInstallCompleted="isPyInstallCompleted"
      :selectToolsSetup="selectToolsSetup"
    />
    <ToolsSkipStep
      v-if="selected === 'manual'"
      :isPyInstallCompleted="isPyInstallCompleted"
      :selectToolsSetup="selectToolsSetup"
    />

    <div
      id="tools-complete-setup"
      v-if="selected === 'complete'"
      key="complete"
      class="section centerize"
    >
      <h2 class="subtitle">
        ESP-IDF Tools have been configured for this extension of Visual Studio
        Code.
      </h2>
      <div class="control">
        <button class="button" href="#" v-on:click="getExamplesList">
          View ESP-IDF project examples!
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { Action, Mutation, State } from "vuex-class";
import ToolsDownloadStep from "./components/ToolsDownloadStep.vue";
import ToolsSkipStep from "./components/ToolsSkipStep.vue";

@Component({
  components: {
    ToolsDownloadStep,
    ToolsSkipStep,
  },
})
export default class ToolsSetup extends Vue {
  @State("toolsSelectedSetupMode") private storeToolsSelectedSetupMode;
  @State("isPyInstallCompleted") private storeisPyInstallCompleted: string;
  @Mutation private setToolSetupMode;

  @Action private getExamplesList;
  @Action private getRequiredTools;
  @Action private saveCustomPathsEnvVars;

  get selected() {
    return this.storeToolsSelectedSetupMode;
  }
  get isPyInstallCompleted() {
    return this.storeisPyInstallCompleted;
  }

  public selectToolsSetup(installType) {
    if (installType === "complete") {
      this.saveCustomPathsEnvVars();
    } else if (
      installType === "auto" ||
      (this.selected === "empty" && installType === "manual")
    ) {
      this.getRequiredTools();
    }
    this.setToolSetupMode(installType);
  }
}
</script>

<style></style>
