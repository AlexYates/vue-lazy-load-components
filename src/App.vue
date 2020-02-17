<template>
	<div id="app">
		<div>
			<button @click="toggleOne">Toggle One</button>
			<button @click="toggleTwo">Toggle Two</button>
		</div>
		<div>
			<template v-if="showOne">
				<One />
			</template>
			<template v-if="showTwo">
				<Two />
			</template>
		</div>
	</div>
</template>

<script>
// import One from '@/components/One.vue'
// import Two from '@/components/Two.vue'

export default {
	components: {
		// One,
		// Two
		One: () => import('@/components/One.vue'),
		Two: () => import('@/components/Two.vue')
	},
	data: () => ({
		showOne: false,
		showTwo: false,
	}),
	methods: {
		toggleOne() {
			this.showOne = !this.showOne
		},
		toggleTwo() {
			this.showTwo = !this.showTwo
		}
	}
}
</script>
