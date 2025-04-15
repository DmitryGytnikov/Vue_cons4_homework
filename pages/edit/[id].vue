<script setup>
const route = useRoute()
console.log(route.params.id)
</script>
<template>
	edit {{ $route.params.id }}
	<Component2 />
</template>
