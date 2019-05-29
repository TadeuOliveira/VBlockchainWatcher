<template>
	<div class="blockinfo">
			<h2>Details from Block {{ $route.params.id }}</h2>
			<router-link to="/">Go back to listing</router-link>	
			<hr>
			<b-row>
				<b-col v-for="row in block">{{ row }}</b-col>
				<b-col v-for="row in block">{{ row }}</b-col>
			</b-row>
	</div>
</template>

<script>

  import Web3 from 'web3'

	export default {
	  name: 'BlockInfo',
		data() {
			return {
				block: []
			}
		},
		props: {

		},
		mounted() {
			this.getData()
		},
		methods: {
			async getData() {
				const web3 = new Web3('http://127.0.0.1:7545')
				block = await web3.eth.getBlock(this.$route.params.id)
				console.log(block)
			}
		}
	}
</script>