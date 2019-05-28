<template>
	<div>
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
        items: [
          { number: 1, isActive: true, age: 40, first_name: 'Dickerson', last_name: 'Macdonald'},
          { number: 2, isActive: false, age: 21, first_name: 'Larsen', last_name: 'Shaw'},
          { number: 3, isActive: false, age: 89, first_name: 'Geneva', last_name: 'Wilson'},
          { number: 4, isActive: true, age: 38, first_name: 'Jami', last_name: 'Carney'}
        ]
      }
    },
    mounted() {
    	this.testNetwork()
    },
    methods: {
    	async testNetwork() {
        const web3 = new Web3("http://127.0.0.1:7545")

        const latest = await web3.eth.getBlockNumber()
        const blockNumbers = [...Array(latest).keys()]
        const batch = new web3.eth.BatchRequest()

        var callback = (err, res) => { err ? res : err }
        var timestampToDate = (t) => { return dateFormat(new Date(t),'dd/mm H:mm:ss') }

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
        //console.log(this.blocks)
        /*const subscription = web3.eth.subscribe('logs', {
          fromBlock: 0
        }, function(error, result){
          if (!error)
            console.log(result);
        })*/
      }
    }
  }
</script>