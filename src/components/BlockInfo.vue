<template>
	<div class="blockinfo">
			<h2>Details from Block {{ $route.params.id }}</h2>
			<router-link to="/">Go back to listing</router-link>	
			<b-table bordered stacked :items="block" style="word-break: break-all;"></b-table>
	</div>
</template>

<script>

  import Web3 from 'web3'

	export default {
	  name: 'BlockInfo',
		data() {
			return {
				block: [],
				fields: ['difficulty','hash'],
				items: [
          { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
          { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
          { age: 89, first_name: 'Geneva', last_name: 'Wilson' }
        ]
			}
		},
		props: {

		},
		mounted() {
			this.getData()
		},
		methods: {
			async getData() {
				const web3 = new Web3(Web3.givenProvider)
				this.block = await web3.eth.getBlock(this.$route.params.id).then(
					(x) => [x])
				console.log(this.block)
			}
		}
	}
</script>