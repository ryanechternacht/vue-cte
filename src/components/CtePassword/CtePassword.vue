<template>
  <base-cte
    :value="internalValue"
    :allow-cancel="allowCancel"
    :require-confirmation="requireConfirmation"
    @editingCompleted="editingCompleted"
    @focusInput="focusInput"
  >
    <template #display-component>
      ********
    </template>

    <template #edit-component>
      <input
        ref="input"
        v-model="internalValue"
        class="cte-password__input"
        type="password"
        v-bind="$attrs"
      >
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
        this.$emit('editingCompleted', { committed })
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
.cte-password__input {
  font-size: inherit;
  font-family: inherit;
  width: 100%;
  box-sizing: border-box;
}

>>> .cte-base__label {
  padding: 3px 4px;
}
</style>
