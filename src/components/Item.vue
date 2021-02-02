<template>
  <div style="height:40px;border: 1px solid red;">
    {{ item.id }}
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