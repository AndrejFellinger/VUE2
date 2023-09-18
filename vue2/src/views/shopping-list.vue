<template>
	<h1>Lists</h1>

	<template v-if="!test1">
		<p>Načítavam dáta</p>
	</template>

	<template v-else-if="test1.error">
		Pri načítaní dát nastala chyba: {{ test1.error }}
	</template>

	<template v-else>
		<div v-for="test1 in test1" :key="`shopping-list-cart-${test1.id}`">
			<a :href="`/shopping-lists/${test1.id}`" @click.prevent="openShoppingListDetail(test1)">
				{{ test1.title }}
			</a>
		</div>
	</template>
</template>

<script>
import axios from 'axios'

export default {
	data() {
		return {
			shoppingLists: null,
            test1: null
		}
	},

	async mounted() {
		try {
			// const response = await axios.get('https://shoppinglist.wezeo.dev/shoppinglist/lists')
			// const data = response.data.data

			// Skrateny zapis zakomentovaneho kodu vyssie, kde vytiahneme data pomocou destrukcie objektu
			const { data: { data: test1} } = await axios.get('/api/v1/shopping-lists')
			this.test1 = test1
		} catch (error) {
			console.error('Error:', error)
			this.test1 = { error }
		}
	},

	methods: {
		openShoppingListDetail({ id }) {
			this.$router.push({ name: 'Shopping List - Detail', params: { id } })
		}
	}
}
</script>