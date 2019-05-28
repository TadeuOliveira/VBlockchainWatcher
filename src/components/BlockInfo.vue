<template>
	<div class="blockinfo">
			<h1>This is Block Info page</h1>
			<h2>the id is {{ $route.params.id }}</h2>
			<router-link to="/blocks">Go to listing</router-link>	
	</div>
</template>

<script>
export default {
  name: 'BlockInfo',
  props: {
		number: Number
  }
}
</script>