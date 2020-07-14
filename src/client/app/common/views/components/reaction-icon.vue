<template>
<mk-emoji :emoji="reaction.startsWith(':') ? null : _reaction" :name="reaction.startsWith(':') ? reaction.substr(1, reaction.length - 2) : null" :is-reaction="true" :customEmojis="customEmojis" :normal="true" :no-style="noStyle"/>
</template>

<script lang="ts">
import Vue from 'vue';
import getReactionEmoji from '../../../../../misc/get-reaction-emoji';
export default Vue.extend({
	props: {
		reaction: {
			type: String,
			required: true
		},
		customEmojis: {
			required: false,
			default: () => []
		},
		noStyle: {
			type: Boolean,
			required: false,
			default: false
		},
	},

	computed: {
		_reaction() {
			return getReactionEmoji(this.reaction)
		}
	}
});
</script>
