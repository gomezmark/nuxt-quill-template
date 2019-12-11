<template>
    <div style="width:100%">
    <v-dialog
      v-model="dialog" 
      persistent
			fullscreen hide-overlay transition="dialog-bottom-transition"
      scrollable
    >
      <v-card tile>
        <v-toolbar
						flat
						>
						<v-btn icon 
							@click="dialog = false">
							<v-icon>chevron_left</v-icon>
						</v-btn>
						<v-toolbar-title>Edit Content</v-toolbar-title>
						<v-spacer></v-spacer>
						<v-toolbar-items>
							<v-btn 
								color="success"
								@click="setTextEditorContent">Save</v-btn>
						</v-toolbar-items>
					</v-toolbar>
        <v-card-text class="relative pa-0">
					<v-layout row wrap>
            <v-flex xs12>
              <v-card>
                <v-card-text>
                  <div ref="editor-toolbar">
                    <span class="ql-formats">
                      <select class="ql-font"></select>
                      <select class="ql-size"></select>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-bold"></button>
                      <button class="ql-italic"></button>
                      <button class="ql-underline"></button>
                      <button class="ql-strike"></button>
                    </span>
                    <span class="ql-formats">
                      <select class="ql-color"></select>
                      <select class="ql-background"></select>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-script" value="sub"></button>
                      <button class="ql-script" value="super"></button>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-header" value="1"></button>
                      <button class="ql-header" value="2"></button>
                      <button class="ql-blockquote"></button>
                      <button class="ql-code-block"></button>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-list" value="ordered"></button>
                      <button class="ql-list" value="bullet"></button>
                      <button class="ql-indent" value="-1"></button>
                      <button class="ql-indent" value="+1"></button>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-direction" value="rtl"></button>
                      <select class="ql-align"></select>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-link"></button>
                      <button class="ql-image"></button>
                      <button class="ql-video"></button>
                      <button class="ql-formula"></button>
                    </span>
                    <span class="ql-formats">
                      <button class="ql-clean"></button>
                    </span>
                    <span class="ql-formats">
                      <button id="insert-table" @click="insertTable">Table</button>
                    </span>

                  </div>
                  <div ref="quill-editor"></div>
                </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
let Quill = null;
let QuillBetterTable = null;
if (process.client || process.browser) {
  Quill = require('quill')
  QuillBetterTable = require('quill-better-table')
  Quill.register({
    'modules/better-table': QuillBetterTable
  }, true)
}

export default {
  data () {
    return {
      dialog: false,
      content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed quis augue vel ex commodo placerat at nec purus. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Duis vehicula justo odio, nec lobortis risus tempor ut. Ut hendrerit ipsum vel lectus convallis efficitur. Mauris blandit tristique sapien a dictum. Maecenas eu dolor ac urna accumsan condimentum et vel arcu. Suspendisse at diam at tortor blandit vulputate. Nullam id eleifend ligula. Etiam pretium fermentum metus eget vehicula. Mauris non sollicitudin massa. Mauris cursus ut mauris sed efficitur. Donec tincidunt condimentum turpis sed ultrices. Duis in metus pharetra, vestibulum tortor sit amet, aliquet elit. Duis at lorem laoreet, imperdiet urna ut, ullamcorper urna.`
    }
  },
  methods: {
    open (value) {
      this.dialog = true
      this.setContent(value, true)
			return new Promise((resolve, reject) => {
        this.resolve = resolve
        this.reject = reject
      })
    },
    insertTable () {
      console.log(this.editorInstance.getModule('better-table'))
      let betterTable = this.editorInstance.getModule('better-table')
      betterTable.insertTable(3, 3)
    },
    setTextEditorContent () {
			this.resolve(this.getContent())
			this.dialog = false
		},
    getContent () {
      return this.$refs['quill-editor'].querySelector('.ql-editor').innerHTML
    },
    setContent (value) {
      this.$refs['quill-editor'].querySelector('.ql-editor').innerHTML = value
    }
  },
  mounted () {
    this.editorInstance = new Quill(this.$refs['quill-editor'], {
      theme: 'snow',
      modules: {
        toolbar: this.$refs['editor-toolbar'],
        table: false,  // disable table module
        'better-table': {
          operationMenu: {
            items: {
              unmergeCells: {
                text: 'Another unmerge cells name'
              }
            }
          }
        },
        keyboard: {
          bindings: QuillBetterTable.keyboardBindings
        }
      },
      placeholder: 'Compose an epic...',
    })
  }
}
</script>
<style scoped>

</style>

