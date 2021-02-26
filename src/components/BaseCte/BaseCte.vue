<template>
  <div
    v-click-outside="clickedAway"
    class="cte-base__container"
    @keydown.esc="editingCanceled"
  >
    <div
      v-if="!editing"
      class="cte-base__label"
      @click="startEditing"
    >
      <slot name="display-component">
        {{ value }}
      </slot>
    </div>
    <div
      v-else
      class="cte-base__input"
    >
      <slot name="edit-component" />
      <button
        v-if="requireConfirmation"
        class="cte-base__button cte-base__button--confirm"
        @click="editingConfirmed"
      />
      <button
        v-if="showCancel"
        class="cte-base__button cte-base__button--cancel"
        @click="editingCanceled"
      />
    </div>
  </div>
</template>

<script>
import ClickOutside from 'vue-click-outside'

export default {
  directives: {
    ClickOutside
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
      editing: false
    }
  },
  computed: {
    showCancel () {
      return this.allowCancel || this.requireConfirmation
    }
  },
  methods: {
    startEditing () {
      this.editing = true
      this.$nextTick(() => {
        this.$emit('focusInput')
      })
    },
    clickedAway () {
      if (!this.editing || this.requireConfirmation) {
        return
      }

      this.editing = false
      this.editingCompleted(true)
    },
    editingCanceled () {
      this.editing = false
      this.editingCompleted(false)
    },
    editingConfirmed () {
      this.editing = false
      this.editingCompleted(true)
    },
    editingCompleted (committed) {
      this.$emit('editingCompleted', { committed })
    }
  }
}
</script>

<style scoped>
.cte-base__container {
  display: inline-block;
  font-size: 16px;
  font-family: Times;
}

.cte-base__label {
  font-size: inherit;
  font-family: inherit;
}

.cte-base__label:hover {
  text-decoration: underline;
}

.cte-base__input {
  display: flex;
  align-items: center;
}

.cte-base__button {
  margin-left: 8px;
  padding: 3px 6px;
  border: 1px solid #bbb;
  border-radius: 4px;
}

.cte-base__button:hover {
  background-color: #ddd;
  cursor: pointer;
}

.cte-base__button--cancel::after {
  color: red;
  content: "✘";
}

.cte-base__button--confirm::after {
  color: green;
  content: "✔";
}
</style>
