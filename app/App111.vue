<template>
  <div>
    <h1>{{a}}</h1>
    <button @click="a++">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      a: 20
    };
  }
};
</script>

<style lang="scss" scoped>
</style>