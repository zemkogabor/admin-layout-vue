<template>
  <i v-if="iconClass" :class="iconClass" />
  {{ label }}
</template>

<script>
export default {
  name: 'SidebarSubItem',
  props: {
    label: {
      type: String,
      required: true,
    },
    iconClass: {
      type: String,
      default: null,
    },
  }
}
</script>
