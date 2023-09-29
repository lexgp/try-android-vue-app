<template>
  <el-menu
    class="el-menu-demo"
    mode="horizontal"
  >
    <el-menu-item index="1">
      <div class="flex items-center">
        <el-avatar
          :size="48"
          class="mr-3"
          src="/vocaloid.png"
        />
        <h2 class="text-large font-600 mr-3 mt-0 mb-0">Калькунятор</h2>
      </div>

    </el-menu-item>
  </el-menu>
  <Home />
</template>

<script setup lang="ts">
  import Home from './components/Home.vue'
</script>
