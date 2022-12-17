<template>
	<div class="vac-message-files-container">
		<div v-for="(file, i) in imageVideoFiles" :key="i + 'iv'">
			<message-file
				:file="file"
				:current-user-id="currentUserId"
				:message="message"
				:index="i"
				:message-selection-enabled="messageSelectionEnabled"
				@open-file="$emit('open-file', $event)"
			>
				<template v-for="(idx, name) in $slots" #[name]="data">
					<slot :name="name" v-bind="data" />
				</template>
			</message-file>
		</div>

		<div v-for="(file, i) in otherFiles" :key="i + 'ivs'" @click="openFile($event, file, 'download')">
			<message-other-files
				:file="file"
				:current-user-id="currentUserId"
				:message="message"
				:index="i"
				:message-selection-enabled="messageSelectionEnabled"
			>
			</message-other-files>
		</div>

		<format-message
			:message-id="message._id"
			:content="message.content"
			:users="roomUsers"
			:text-formatting="textFormatting"
			:link-options="linkOptions"
			@open-user-tag="$emit('open-user-tag', $event)"
		/>
	</div>
</template>

<script>
import SvgIcon from '../../../../components/SvgIcon/SvgIcon'
import FormatMessage from '../../../../components/FormatMessage/FormatMessage'
import ProgressBar from '../../../../components/ProgressBar/ProgressBar'

import MessageFile from './MessageFile/MessageFile'

import { isImageVideoFile } from '../../../../utils/media-file'

export default {
	name: 'MessageFiles',
	components: { SvgIcon, FormatMessage, ProgressBar, MessageFile },

	props: {
		currentUserId: { type: [String, Number], required: true },
		message: { type: Object, required: true },
		roomUsers: { type: Array, required: true },
		textFormatting: { type: Object, required: true },
		linkOptions: { type: Object, required: true },
		messageSelectionEnabled: { type: Boolean, required: true }
	},

	emits: ['open-file', 'open-user-tag'],

	data() {
		return {
			imageResponsive: '',
		}
	},

	computed: {
		imageVideoFiles() {
			return this.message.files.filter(file => isImageVideoFile(file))
		},
		otherFiles() {
			return this.message.files.filter(file => !isImageVideoFile(file))
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
		openFile(event, file, action) {
			if (!this.messageSelectionEnabled) {
				event.stopPropagation()
				this.$emit('open-file', { file, action })
			}
		}
	}
}
</script>
