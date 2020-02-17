# How to lazy load components with Vue

## Install Vue CLI

> `npm i -g @vue/cli`

## Create a new application

> `vue create vue-lazy-loading-components`

Accept the defaults for this demo, note that we're only going to be running this demo with the built-in local and not optimising for production.

## Run the local development server

> `npm run serve`

## Some clean-up & component code

We'll have four components in total, with the latter two being child and grandchild components, and they'll all only be shown when the user clicks their respective buttons.

Initally lets load them synchronously.

> `One.vue`
```javascript
<template>
	<div>
		<span>One</span>
	</div>
</template>
```

> `Two.vue`
```javascript
<template>
	<div>
		<span>Two</span>
		<Three />
	</div>
</template>

<script>
import Three from '@/components/Three.vue'

export default {
	components: {
		Three
	}
}
</script>
```

> `Three.vue`
```javascript
<template>
	<div>
		<span>Three</span>
		<Four />
	</div>
</template>

<script>
import Four from '@/components/Four.vue'

export default {
	components: {
		Four
	}
}
</script>
```

> `Four.vue`
```javascript
<template>
	<div>
		<span>Four</span>
	</div>
</template>
```

> `App.vue`
```javascript
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
import One from '@/components/One.vue'
import Two from '@/components/Two.vue'

export default {
	components: {
		One,
		Two
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
```

## Verifying the results

With your browsers Network tab active, open the development website at http://localhost:3000 , notice the `app.js` file which contains your all your sites code including those three components, it's probably around `110KB`.

Click on the `Toggle One` & `Two` buttons to show the respective components `One`, `Two` and child component `Three` and `Four`, note that no extra network traffic occurs as all the components are contained within `app.js`.

## Lazy load those components

> `App.vue`
```diff
--import One from '@/components/One.vue'
--import Two from '@/components/Two.vue'

--		One,
--		Two
++		One: () => import('@/components/One.vue'),
++		Two: () => import('@/components/Two.vue')
```

> `Two.vue`
```diff
--import Three from '@/components/Three.vue'

++		Three: () => import('@/components/Three.vue')
```

> `Three.vue`
```diff
--import Four from '@/components/Four.vue'

++		Four: () => import('@/components/Four.vue')
```

Now refresh the page and notice the `app.js` file is now down to roughly `60KB`, now click the `Toggle` buttons and observe the Network calls for `0.js`, `1.js`, `2.js` and `3.js` for the components `One`, `Two` and child component `Three` and `Four`; each between `12` to `18KB`.

## Is it worth it

For components as trivial as this demo, no.
Less contrived scenarios where a component may not need to be fully instantiated immediately and where users would benefit in seeing other more primary features sooner, yes.

As you can imagine, A/B testing these scenarios is incredibly simple too; only a few lines of code needs to be altered meaning you have a clean A/B test to derive accurate data from.

## Similar articles elsewhere

- https://dev.to/angular/angular-9-lazy-loading-components-o04
