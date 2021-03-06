<template>
  <div
    class="demo-frame"
    :class="{ 'is-inline': inline, 'is-loading': isLoading }"
    v-observe-visibility="{
      callback: visibilityChanged,
      once: true,
    }"
  >
    <div class="demo-frame__loader-wrapper" v-if="isLoading">
      <div class="demo-frame__loader" />
    </div>
    <iframe
      class="demo-frame__iframe"
      v-resize.quiet="{ scrolling: 'omit' }"
      :src="`/demos/${name}?${query}`"
      width="100%"
      height="0"
      frameborder="0"
      @load="onLoad"
      v-if="isVisible"
    />
  </div>
</template>

<script>
import { ObserveVisibility } from 'vue-observe-visibility'

export default {
  props: {
    name: {
      type: String,
      required: true,
    },

    inline: {
      type: Boolean,
      default: false,
    },

    highlight: {
      type: String,
      default: null,
    },

    showSource: {
      type: Boolean,
      default: true,
    },
  },

  directives: {
    ObserveVisibility,
  },

  data() {
    return {
      isLoading: true,
      isVisible: false,
    }
  },

  computed: {
    query() {
      return `inline=${this.inline}&highlight=${this.highlight}&showSource=${this.showSource}`
    },
  },

  methods: {
    onLoad() {
      this.isLoading = false
    },

    visibilityChanged(isVisible) {
      this.isVisible = isVisible
    },
  },
}
</script>

<style lang="scss" scoped>
.demo-frame {
  display: flex;
  flex-direction: column;
  position: relative;
  min-height: 5rem;

  &__iframe {
    background-color: transparent;
    max-height: 100%;
  }

  &.is-inline {
    border-radius: 0.75rem;
    background-color: rgba($colorBlack, 0.03);
    margin: -1.25rem;
  }

  &__loader-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    pointer-events: none;
  }

  &__loader {
    font-size: 10px;
    position: relative;
    text-indent: -9999rem;
    border-top: 2px solid rgba($colorBlack, 0.2);
    border-right: 2px solid rgba($colorBlack, 0.2);
    border-bottom: 2px solid rgba($colorBlack, 0.2);
    border-left: 2px solid $colorBlack;
    transform: translateZ(0);
    animation: load8 1.1s infinite linear;

    &,
    &::after {
      border-radius: 50%;
      width: 1.25rem;
      height: 1.25rem;
    }
  }

  @keyframes load8 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }

    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
}
</style>
