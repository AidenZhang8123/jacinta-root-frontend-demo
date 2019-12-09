<template>
  <section class="container" > sss<!--最外层容器-->
    <section class="content-container"><!--右边的容器-->
    <transition name="fade-transform" mode="out-in">
      <router-view :key="key" />
    </transition>
    </section>
  </section>
</template>
<script>
  export  default {};
</script>

<style lang="scss" scoped>
// @import '../common/style/layouts.less';
</style>
