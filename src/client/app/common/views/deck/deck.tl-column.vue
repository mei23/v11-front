<template>
<x-column :menu="menu" :name="name" :column="column" :is-stacked="isStacked">
	<template #header>
		<fa v-if="column.type == 'home'" icon="home"/>
		<fa v-if="column.type == 'local'" :icon="['far', 'comments']"/>
		<fa v-if="column.type == 'hybrid'" icon="share-alt"/>
		<fa v-if="column.type == 'global'" icon="globe"/>
		<fa v-if="column.type == 'list'" icon="list"/>
		<fa v-if="column.type == 'antenna'" icon="satellite"/>
		<fa v-if="column.type == 'hashtag'" icon="hashtag"/>
		<span>{{ name }}</span>
	</template>

	<div class="editor" style="padding:12px" v-if="edit">
		<ui-switch v-model="column.isMediaOnly" @change="onChangeSettings">{{ $t('is-media-only') }}</ui-switch>
	</div>

	<x-list-tl v-if="column.type == 'list'"
		:list="column.list"
		:media-only="column.isMediaOnly"
		ref="tl"
	/>
	<x-antenna-tl v-else-if="column.type == 'antenna'"
		:antenna="column.antenna"
		:media-only="column.isMediaOnly"
		ref="tl"
	/>
	<x-hashtag-tl v-else-if="column.type == 'hashtag'"
		:tag-tl="$store.state.settings.tagTimelines.find(x => x.id == column.tagTlId)"
		:media-only="column.isMediaOnly"
		ref="tl"
	/>
	<x-tl v-else
		:src="column.type"
		:media-only="column.isMediaOnly"
		ref="tl"
	/>
</x-column>
</template>

<script lang="ts">
import Vue from 'vue';
import i18n from '../../../i18n';
import XColumn from './deck.column.vue';
import XTl from './deck.tl.vue';
import XListTl from './deck.list-tl.vue';
import XAntennaTl from './deck.antenna-tl.vue';
import XHashtagTl from './deck.hashtag-tl.vue';

export default Vue.extend({
	i18n: i18n('deck/deck.tl-column.vue'),
	components: {
		XColumn,
		XTl,
		XListTl,
		XAntennaTl,
		XHashtagTl
	},

	props: {
		column: {
			type: Object,
			required: true
		},
		isStacked: {
			type: Boolean,
			required: true
		}
	},

	data() {
		return {
			edit: false,
			menu: [{
				icon: 'cog',
				text: this.$t('edit'),
				action: () => {
					this.edit = !this.edit;
				}
			}]
		}
	},

	computed: {
		name(): string {
			if (this.column.name) return this.column.name;

			switch (this.column.type) {
				case 'home': return this.$t('@deck.home');
				case 'local': return this.$t('@deck.local');
				case 'hybrid': return this.$t('@deck.hybrid');
				case 'global': return this.$t('@deck.global');
				case 'list': return this.column.list.name;
				case 'antenna': return this.column.antenna.name;
				case 'hashtag': return this.$store.state.settings.tagTimelines.find(x => x.id == this.column.tagTlId).title;
				default: return '';
			}
		}
	},

	methods: {
		onChangeSettings(v) {
			this.$store.commit('updateDeckColumn', this.column);
		},

		focus() {
			this.$refs.tl.focus();
		}
	}
});
</script>
