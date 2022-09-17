<template>
  <main font-sans p="x-4 y-4" class="main" text="gray-700 dark:gray-200">
    <router-view />
    <Footer />
  </main>
</template>

<style scoped>
  .main {
    @apply text-center;
  }
</style>
