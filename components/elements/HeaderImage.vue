<template>
  <!-- <section class="header-image w-100" :style="{ backgroundImage: `url(${src})` }"> -->
  <section class="header-image">
    <span class="text-white">{{ title }}</span>
    <!-- <ElementsImage :classes="['responsive']" :src="src" /> -->
    <!-- <ElementsImage :classes="['responsive']" 
    :width="`100%`" :height="`400px`"/> -->
  </section>
</template>

<script setup>
const { src, title } = defineProps(["src", "title"]);
</script>

<style lang="scss" scoped>
.header-image {
  width: 100vw;
  background-size: contain;
  background-repeat: no-repeat;
  height: 70px;
  // padding: 50px 30px;
  span {
    font-size: 25px;
    background: #14aabb;
    height: 100%;
    padding: 20px 50px;
    border-bottom-right-radius: 30px;
  }
}
</style>
