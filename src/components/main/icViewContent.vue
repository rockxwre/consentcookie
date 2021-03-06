<!--
  - Copyright 2018 Asknow Solutions B.V.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -
  -->

<template>
  <div id="ic-view-content">
    <div :style="scrollholderCSS" class="ic-view-scollholder">
      <div :style="scrollContentCSS" class="ic-view-scrollcontent">
        <slot/>
      </div>
    </div>
  </div>
</template>

<script>

  // Defaults
  // http://www.textfixer.com/tutorials/browser-scrollbar-width.php
  const DEFAULT_SCROLLBAR_WIDTH = 17;

  // private variables
  let scrollContent;
  let scrollContentObserver;

  const data = {
    scrollholderCSS: {
      /* to hide the scrollbar we want a negative margin */
      marginRight: -DEFAULT_SCROLLBAR_WIDTH + 'px',
    },
    scrollContentCSS: {
      /* to align content properly, undo the negagive margin */
      marginRight: DEFAULT_SCROLLBAR_WIDTH + 'px',
    },
  };

  // private functions
  function initContentChangeListener() {
    const vue = this;
    // Get reference to the scrollcontent element
    scrollContent = vue.$el.querySelector('.ic-view-scrollcontent');
    scrollContentObserver = new MutationObserver(((mutations, observer) => {
      const contentUpdate = { content: { size: scrollContent.scrollHeight } };
      vue.$services.view.notifyViewChanged(contentUpdate);
    }));
    scrollContentObserver.observe(scrollContent, {
      childList: true,
      subtree: true,
    });
  }

  // Vue module
  module.exports = {
    name: 'icViewContent',
    components: {},
    data() {
      return data;
    },
    methods: {},
    mounted() {
      initContentChangeListener.call(this);
    },
  };
</script>

<style lang="scss" scoped>

  @import '../../assets/scss/general-variables';

  #ic-view-content {

    border: $ic-box-border;
    border-radius: 0px 0px $ic-box-border-radius $ic-box-border-radius;
    background: $ic-color-white;

    .ic-view-scollholder {

      height: 100%;
      margin-bottom: 5px;
      overflow-x: auto;
      overflow-y: scroll;

      .ic-view-scrollcontent {
        overflow: hidden;
      }
    }
  }
</style>
