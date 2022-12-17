<template>
	<div class="vac-message-file-container">
		<div
			:ref="'imageRef' + index"
			class="vac-message-image-container"
		>
			<div
				class="vac-message-image"
				:style="{
					'background-image': `url('${file.preview}')`,
					'max-height': `${imageResponsive.maxHeight}px`
				}"
			>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'MessageOtherFiles',
	props: {
		currentUserId: { type: [String, Number], required: true },
		message: { type: Object, required: true },
		file: { type: Object, required: true },
		index: { type: Number, required: true },
		messageSelectionEnabled: { type: Boolean, required: true }
	},

	data() {
		return {
			imageResponsive: '',
		}
	},

	mounted() {
		const ref = this.$refs['imageRef' + this.index]

		if (ref) {
			this.imageResponsive = {
				maxHeight: ref.clientWidth - 18,
				loaderTop: ref.clientHeight / 2 - 9
			}
		}
	},

	methods: {
	}
}
</script>
