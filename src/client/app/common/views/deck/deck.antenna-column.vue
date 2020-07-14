<template>
<x-column>
	<template #header>
		<fa icon="satellite"/><span>{{ tag }}</span>
	</template>

	<div class="astecv9w">
		<x-antenna-tl :antenna="antenna" class="tl" :key="antenna.id"/>
	</div>
</x-column>
</template>

<script lang="ts">
import Vue from 'vue';
import XColumn from './deck.column.vue';
import XAntennaTl from './deck.antenna-tl.vue';

export default Vue.extend({
	components: {
		XColumn,
		XAntennaTl
	},

	watch: {
		$route: 'fetch'
	},
});
</script>

<style lang="stylus" scoped>
.astecv9w
	background var(--deckColumnBg)

	> .tl
		background var(--face)

</style>
