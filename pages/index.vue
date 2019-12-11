<template>
  <div>
    <no-ssr>
      <editor ref="Editor"></editor>
    </no-ssr>
    <v-layout row justify-center align-center>
      <v-flex xs12 sm8 md6>
        <div class="title mb-3">Sample Document</div>
        <v-card>
          <v-card-text>
            <div v-html="content"></div>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="editContent">Edit Content</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import Editor from '~/components/Editor.vue'
export default {
  data () {
    return {
      content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed quis augue vel ex commodo placerat at nec purus. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Duis vehicula justo odio, nec lobortis risus tempor ut. Ut hendrerit ipsum vel lectus convallis efficitur. Mauris blandit tristique sapien a dictum. Maecenas eu dolor ac urna accumsan condimentum et vel arcu. Suspendisse at diam at tortor blandit vulputate. Nullam id eleifend ligula. Etiam pretium fermentum metus eget vehicula. Mauris non sollicitudin massa. Mauris cursus ut mauris sed efficitur. Donec tincidunt condimentum turpis sed ultrices. Duis in metus pharetra, vestibulum tortor sit amet, aliquet elit. Duis at lorem laoreet, imperdiet urna ut, ullamcorper urna.'
    }
  },
  components: {
    Editor
  },
  methods: {
    editContent () {
      this.$refs['Editor'].open(this.content)
        .then(result => this.content = result)
    }
  }
}
</script>
