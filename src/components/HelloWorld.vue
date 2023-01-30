<template>
	<BubbleMenu v-if="editor" :editor="editor" :tippy-options="{ duration: 100 }">
		<button @click="editor.chain().focus().toggleBold().run()" :class="{ 'is-active': editor.isActive('bold') }">
		bold
		</button>
		<button @click="editor.chain().focus().toggleItalic().run()" :class="{ 'is-active': editor.isActive('italic') }">
		italic
		</button>
		<button @click="editor.chain().focus().toggleStrike().run()" :class="{ 'is-active': editor.isActive('strike') }">
		strike
		</button>
	</BubbleMenu>

	<div class="content-container">
		<editor-content :editor="editor" class="editor" />
	</div>
</template>

<script>
import { Editor, EditorContent, BubbleMenu } from '@tiptap/vue-3'
import { SearchNReplace } from '@sereneinserenade/tiptap-extension-search-n-replace'
import StarterKit from '@tiptap/starter-kit'
import Typography from '@tiptap/extension-typography'
import Placeholder from '@tiptap/extension-placeholder'

export default {
	components: { EditorContent, BubbleMenu },

	props: {
		content:[String, Object],
	},

	data() {
		return {
			editor: null,
		}
	},

	mounted() {
		this.editor = new Editor({
			content: this.content,
			extensions: [
				SearchNReplace,
				StarterKit.configure({
					heading: {
						levels: [1, 2, 3],
					},
				}),
				Typography.configure({
					openDoubleQuote: false,
					closeDoubleQuote: false,
					openSingleQuote: false,
					closeSingleQuote: false,
				}),
				Placeholder.configure({
					placeholder: this.placeholder || '',
				}),
			],
			autofocus: true
		})
	},

	beforeUnmount() {
		this.editor.destroy()
	},
}
</script>

<style scoped>
.content-container {
	width: 50vh;
	border: 2px solid #666;
	border-radius: 4px;
}
</style>