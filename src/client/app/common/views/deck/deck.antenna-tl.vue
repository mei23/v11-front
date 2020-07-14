<template>
<x-notes ref="timeline" :pagination="pagination" @inited="() => $emit('loaded')"/>
</template>

<script lang="ts">
import Vue from 'vue';
import XNotes from './deck.notes.vue';

export default Vue.extend({
	components: {
		XNotes
	},

	props: {
		antenna: {
			type: Object,
			required: true
		},
		mediaOnly: {
			type: Boolean,
			required: false,
			default: false
		}
	},

	data() {
		return {
			connection: null,
			pagination: {
				endpoint: 'antennas/notes',
				limit: 10,
				params: init => ({
					untilDate: init ? undefined : (this.date ? this.date.getTime() : undefined),
					withFiles: this.mediaOnly,
					includeMyRenotes: this.$store.state.settings.showMyRenotes,
					includeRenotedMyNotes: this.$store.state.settings.showRenotedMyNotes,
					includeLocalRenotes: this.$store.state.settings.showLocalRenotes,
					antennaId: this.antenna.id
				})
			}
		};
	},

	watch: {
		mediaOnly() {
			this.$refs.timeline.reload();
		}
	},

	mounted() {
		if (this.connection) this.connection.close();
		this.connection = this.$root.stream.connectToChannel('antenna', {
			antennaId: this.antenna.id
		});
		this.connection.on('note', this.onNote);
	},

	beforeDestroy() {
		this.connection.dispose();
	},

	methods: {
		onNote(note) {
			if (this.mediaOnly && note.files.length == 0) return;
			(this.$refs.timeline as any).prepend(note);
		},

		focus() {
			this.$refs.timeline.focus();
		}
	}
});
</script>
