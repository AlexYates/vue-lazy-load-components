<template>
	<div>
		<span>Three</span>
		<Four />
	</div>
</template>

<script>
// import Four from '@/components/Four.vue'

export default {
	components: {
		// Four
		Four: () => import('@/components/Four.vue')
	}
}
</script>
