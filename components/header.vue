<template>
  <div class="flex flex-row gap-4">
    <Logo />
    <!-- Internal Link -->
    <NuxtLink to="/" rel="next" class="text-black no-underline">Home</NuxtLink>
    <!-- External Link -->
    <NuxtLink to="/" rel="external" class="text-black no-underline"
      >Other</NuxtLink
    >
  </div>
</template>
