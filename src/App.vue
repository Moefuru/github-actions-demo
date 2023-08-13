<script setup>
import Index from "@/components/Index.vue";
</script>

<template>
  <main>
    <h1 id="tool-name" class="green">在线工具搜索</h1>
    <Index />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
main{
  width: 700px;
}
@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
  }
  #tool-name{
    text-align: center;
    padding: 20px 0;
    font-size: 45px;
  }
}
</style>
