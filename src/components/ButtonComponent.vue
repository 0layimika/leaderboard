<template>
  <div>
    <button ref="trigger" data-action="toggle" @click="togglePopup"
      style="background-color: transparent; border: none; padding: 0;">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"
        style="width: 24px; height: 24px;">
        <path
          d="M5 10C3.9 10 3 10.9 3 12C3 13.1 3.9 14 5 14C6.1 14 7 13.1 7 12C7 10.9 6.1 10 5 10ZM19 10C17.9 10 17 10.9 17 12C17 13.1 17.9 14 19 14C20.1 14 21 13.1 21 12C21 10.9 20.1 10 19 10ZM12 10C10.9 10 10 10.9 10 12C10 13.1 10.9 14 12 14C13.1 14 14 13.1 14 12C14 10.9 13.1 10 12 10Z" />
      </svg>
    </button>

    <div v-if="isOpen" ref="content" class="menu-container">
      <div v-if="!this.params.player.inGame">
        <div class="menu-item" @click="this.params.showChallengers">
          Challenge
        </div>
        <div class="menu-item">
          Dispute
        </div>
      </div>
      <div v-else>
        <div class="menu-item">
          Submit Result
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import tippy from 'tippy.js';
import { hideAll } from 'tippy.js';
import '../assets/style.css'
export default {
  data() {
    return {
      isOpen: false,
      tippyInstance: null,
    };
  },
  mounted() {
    this.tippyInstance = tippy(this.$refs.trigger);
    this.tippyInstance.disable()
  },
  methods: {
    togglePopup() {
      this.isOpen = !this.isOpen;
      if (this.isOpen) {
        this.$nextTick(() => {
          this.configureTippyInstance();
          this.tippyInstance.setContent(this.$refs.content);
        });
      } else {
        this.tippyInstance.unmount();
      }
    },
    configureTippyInstance() {
      this.tippyInstance.enable();
      this.tippyInstance.show();
      this.tippyInstance.setProps({
        trigger: 'manual',
        placement: 'left',
        allowHTML: true,
        arrow: false,
        interactive: true,
        appendTo: document.body,
        hideOnClick: false,
        onShow: (instance) => {
          hideAll({ exclude: instance });
        },
        onClickOutside: (instance) => {
          this.isOpen = false;
          instance.unmount();
        },
      });
    },
     },
  
};
</script>
