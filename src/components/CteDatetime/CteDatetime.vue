<template>
  <base-cte
    class="cte-datetime__container"
    :value="internalValue"
    :allow-cancel="allowCancel"
    :require-confirmation="requireConfirmation"
    @editingCompleted="editingCompleted"
    @focusInput="focusInput"
  >
    <template #display-component>
      {{ dateFormatted }}
    </template>

    <template #edit-component>
      <input
        ref="input"
        v-model="internalValue"
        type="datetime-local"
        class="cte-datetime__input"
        v-bind="$attrs"
      >
    </template>
  </base-cte>
</template>

<script>
import BaseCte from './BaseCte'
import { parseISO, format } from 'date-fns'

export default {
  components: {
    BaseCte
  },
  props: {
    value: {
      type: String,
      default: ''
    },
    displayFormat: {
      type: String,
      default: 'MM/dd/yyyy, hh:mm aa'
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
  computed: {
    dateFormatted () {
      return format(parseISO(this.internalValue), this.displayFormat)
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
.cte-datetime__container {
  width: 250px;
}

.cte-datetime__input {
  font-size: inherit;
  font-family: inherit;
  width: 100%;
  box-sizing: border-box;
}

>>> .cte-base__label {
  padding: 5px 4px;
  letter-spacing: .03em;
}
</style>
