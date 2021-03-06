<template>
  <label @mousedown="handleMouseDown" @mouseup="handleMouseUp" @touchstart="handleTouchStart"
    @touchend="handleTouchEnd" @click.stop="handleClick"
    class="mu-checkbox" :class="{'label-left': labelLeft, 'disabled': disabled, 'no-label': !label}">
    <input type="checkbox" :disabled="disabled" :name="name" :value="nativeValue" v-model="inputValue">
    <touch-ripple v-if="!disabled" rippleWrapperClass="mu-checkbox-ripple-wrapper" class="mu-checkbox-wrapper">
      <div class="mu-checkbox-label"  v-if="label && labelLeft">{{label}}</div>
      <div class="mu-checkbox-icon">
        <icon :value="uncheckIcon" class="mu-checkbox-icon-uncheck"></icon>
        <icon :value="checkedIcon" class="mu-checkbox-icon-checked"></icon>
      </div>
      <div class="mu-checkbox-label"  v-if="label && !labelLeft">{{label}}</div>
    </touch-ripple>
    <div class="mu-checkbox-wrapper" v-if="disabled">
      <div class="mu-checkbox-label"  v-if="label && labelLeft">{{label}}</div>
      <div class="mu-checkbox-icon">
        <icon :value="uncheckIcon" class="mu-checkbox-icon-uncheck"></icon>
        <icon :value="checkedIcon" class="mu-checkbox-icon-checked"></icon>
      </div>
      <div class="mu-checkbox-label"  v-if="label && !labelLeft">{{label}}</div>
    </div>
  </label>
</template>

<script>
import icon from '../icon'
import touchRipple from '../internal/touchRipple'
export default {
  name: 'mu-checkbox',
  props: {
    name: {
      type: String
    },
    value: {},
    nativeValue: {
      type: String
    },
    label: {
      type: String,
      default: ''
    },
    labelLeft: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    uncheckIcon: {
      type: String,
      default: 'check_box_outline_blank'
    },
    checkedIcon: {
      type: String,
      default: 'check_box'
    }
  },
  data () {
    return {
      inputValue: this.value
    }
  },
  watch: {
    value (val) {
      this.inputValue = val
    },
    inputValue (val) {
      this.$emit('input', val)
      this.$emit('change', val)
    }
  },
  methods: {
    handleClick () {
      // 阻止事件冒泡，放置外部控制的时候触发两次 click
    },
    handleMouseDown (event) {
      if (this.disabled) return
      if (event.button === 0) {
        this.$children[0].start(event)
      }
    },
    handleMouseUp () {
      if (this.disabled) return
      this.$children[0].end()
    },
    handleMouseLeave () {
      if (this.disabled) return
      this.$children[0].end()
    },
    handleTouchStart (event) {
      if (this.disabled) return
      this.$children[0].start(event)
    },
    handleTouchEnd () {
      if (this.disabled) return
      this.$children[0].end()
    }
  },
  components: {
    icon,
    'touch-ripple': touchRipple
  }
}
</script>

<style lang="less">
@import "../styles/import.less";
.mu-checkbox {
  position: relative;
  display: inline-block;
  height: 24px;
  line-height: 24px;
  cursor: pointer;
  user-select: none;
  input[type="checkbox"] {
    display: none;
    &:checked {
      + .mu-checkbox-wrapper {
        .mu-checkbox-icon-uncheck{
          opacity: 0;
          transition: opacity 650ms @easeOutFunction 150ms;
          color: @primaryColor;
        }
        .mu-checkbox-icon-checked{
          opacity: 1;
          transform: scale(1);
          transition: opacity 0ms @easeOutFunction, transform 800ms @easeOutFunction;
        }
        .mu-checkbox-ripple-wrapper{
          color: @primaryColor;
        }
      }

    }
  }

  * {
    pointer-events: none;
  }
  &.disabled  {
    cursor: not-allowed;
  }
}

.mu-checkbox-wrapper{
  display: flex;
  width: 100%;
  height: 24px;
  align-items: center;
  justify-content: space-between;
}
.mu-checkbox-icon{
  width: 24px;
  height: 24px;
  vertical-align: middle;
  position: relative;
  margin-right: 16px;
  .mu-checkbox.label-left &{
    margin-right: 0;
    margin-left: 16px;
  }
  .mu-checkbox.no-label &{
    margin-left: 0;
    margin-right: 0;
  }
}

.mu-checkbox-label {
  color: @textColor;
  .mu-checkbox.disabled & {
    color: @disabledColor;
  }
}

.mu-checkbox-icon-uncheck {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 1;
  transition: opacity 1s @easeOutFunction .2s;
  color: @textColor;
  .mu-checkbox.disabled & {
    color: @disabledColor;
  }
}

.mu-checkbox-icon-checked {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  color: @primaryColor;
  transform: scale(0);
  transition: opacity 450ms @easeOutFunction, transform 0ms @easeOutFunction 450ms;
  .mu-checkbox.disabled & {
    color: @disabledColor;
  }
}

.mu-checkbox-ripple-wrapper {
  width: 48px;
  height: 48px;
  top: -12px;
  left: -12px;
  .mu-checkbox.label-left & {
      right: -12px;
      left: auto;
  }
}
</style>
