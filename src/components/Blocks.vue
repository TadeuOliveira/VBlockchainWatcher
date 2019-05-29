<template>
	<div>
		<b-alert v-if="hasProvider" show>Loaded data from the {{ network }} network</b-alert>
		<b-table striped outlined hover :items="blocks">
			<span slot="link" slot-scope="data">
				<router-link :to="'/blocks/' + data.value"><b-button>See Details</b-button></router-link>
			</span>
		</b-table>
	</div>
</template>

<script>
  import Web3 from 'web3'
	import dateFormat from 'dateformat'

  export default {
		name: 'Blocks',
    data() {
      return {
        // Note `isActive` is left out and will not appear in the rendered table
        blocks: [],
        hasProvider: false,
        network: ''
      }
    },
    mounted() {
			this.loadBlocks(5)
    },
    methods: {
			async loadBlocks(limit) {

        const web3 = new Web3(Web3.givenProvider)
        if (!web3.givenProvider) return this.hasProvider = false
        else this.hasProvider = true
        web3.eth.net.getNetworkType().then(x => this.network = x)

        const latest = await web3.eth.getBlockNumber()
        const blockNumbers = latest < limit ? [...Array(latest).keys()] : [...Array(limit).keys()].map((e) => latest - e)
        const batch = new web3.eth.BatchRequest()
        var callback = (res, err) => { err ? err : res }
        var timestampToDate = (t) => { return dateFormat(new Date(t*1000),'dd/mm H:mm:ss') }

        blockNumbers.forEach((blockNumber) => {
          batch.add(
            web3.eth.getBlock.request(blockNumber,callback)
          )
        })

        this.blocks = await batch.execute().then(function(x){
          return x.response.map(x => ({
            'block number': x.number, 
            'hash': x.hash, 
            'size': x.size, 
            'mined at': timestampToDate(x.timestamp),
            'link': x.number }))
        })
      },
      async getNetwork() {
      	console.log('this is network now: ' + this.network)
      }
    }
  }
</script>