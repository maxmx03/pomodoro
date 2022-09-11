<script setup>
import Pomodoro from '@/components/Pomodoro.vue'
</script>

<script>
export default {
  components: {
    Pomodoro,
  },
}
</script>

<template>
  <div class="container">
    <Pomodoro />
  </div>
</template>

<style scoped lang="scss">
.container {
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
