<template>
  <div style="padding: 20px 0;border: 1px solid red;">
    {{ item.value }}
  </div>
</template>

<script>
export default {
  name: 'item',
  props: {
    item: Object
  }
}
</script>

<style>

</style>