<template>
  <div>
    <h1>{{ num }} - {{ name | uppercase }}</h1>
    <small>{{ url }}</small>
  </div>
</template>
<script>
export default {
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    uppercase(value) {
      const newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
};
</script>
<style>
</style>