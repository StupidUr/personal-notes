<!-- default.vue -->
<template>
  <div class="slidev-layout default">
    <slot/>
  </div>
</template>
<style lang="scss">
.default{
  overflow: auto;
}
::-webkit-scrollbar {
  display:none
}
</style>
