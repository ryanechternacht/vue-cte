<template>
  <base-cte
    :value="internalValue"
    :allow-cancel="allowCancel"
    :require-confirmation="requireConfirmation"
    @editingCompleted="editingCompleted"
    @focusInput="focusInput"
  >
    <template #edit-component>
      <select
        ref="input"
        v-model="internalValue"
        class="cte-select__select"
        v-bind="$attrs"
      >
        <slot />
      </select>
    </template>
  </base-cte>
</template>

<script>
import BaseCte from './../BaseCte'

export default {
  components: {
    BaseCte
  },
  props: {
    value: {
      type: String,
      default: ''
    },
    allowCancel: {
      type: Boolean,
      default: false
    },
    requireConfirmation: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      internalValue: this.value
    }
  },
  methods: {
    editingCompleted ({ committed }) {
      if (committed) {
        this.$emit('input', this.internalValue)
        this.$emit('editingCompleted', { committed, newValue: this.internalValue })
      } else {
        this.$emit('editingCompleted', { committed })
      }
    },
    focusInput () {
      this.$refs.input.focus()
    }
  }
}
</script>

<style scoped>
.cte-select__select {
  font-size: inherit;
  font-family: inherit;
  width: 100%;
  box-sizing: border-box;
}

>>> .cte-base__label {
  padding: 3px 4px;
}
</style>
