<template>
	<div>
		<span>Two</span>
		<Three />
	</div>
</template>

<script>
// import Three from '@/components/Three.vue'

export default {
	components: {
		// Three
		Three: () => import('@/components/Three.vue')
	}
}
</script>
