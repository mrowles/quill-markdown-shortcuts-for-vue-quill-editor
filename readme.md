# Quill Markdown Shortcuts for vue-quill-editor

This is a fork of Patrick Lee’s excellent [Markdown Shortcuts](https://github.com/patleeman/quill-markdown-shortcuts) module for [Quill.js](https://quilljs.com) that converts markdown on the fly to formatted rich text. The fork is optimised to work out of the box with [Vue.js](https://vuejs.org), [vue-quill-editor](https://github.com/surmon-china/vue-quill-editor), and [Nuxt](https://nuxtjs.org).

I had to alter Patrick’s original code as I kept getting “Quill is undefined” errors in my app when trying to use it with my above-mentioned setup. I was able to work around them by importing the classes directly and, within those, importing Quill, and exporting the main class via an ES6 module export. (That basically sums up the difference. Apart from that I’ve stripped everything that’s not explicitly necessary, including the Webpack config, etc.)

For general purpose use, and for the canonical location for the original module and related materials, please see [Markdown Shortcuts](https://github.com/patleeman/quill-markdown-shortcuts).

# Using with vue-quill-editor in Nuxt

1. `npm i vue-quill-editor github:aral/quill-markdown-shortcuts-for-vue-quill-editor`

2. In the _plugins_ directory of your Nuxt app, create a _vue-quill-editor.js_ file and add the following code to it:

   ```javascript
    /* Client-side Quill Editor plugin with Markdown Shortcuts */
    import Vue from 'vue'

    import Quill from 'quill'
    import VueQuillEditor from 'vue-quill-editor'

    import 'quill/dist/quill.core.css'
    import 'quill/dist/quill.snow.css'
    import 'quill/dist/quill.bubble.css'

    import MarkdownShortcuts from 'quill-markdown-shortcuts-for-vue-quill-editor'
    Quill.register('modules/markdownShortcuts', MarkdownShortcuts)

    Vue.use(VueQuillEditor)
   ```

3. In your Nuxt configuration, make sure to disable SSR for vue-quill-editor:

    ```javascript
    module.exports = {
      plugins: [
        { src: '~plugins/vue-quill-editor', ssr: false }
      ]
    }
    ```

4. In your page (e.g., _index.vue_ under your _pages_ directory), instantiate the Quill component and specify the Markdown Shortcuts. Example:

    ```html
    <template>
      <section>
        <quill-editor
          v-model='editorContent'
          ref='textEditor'
          :options='editorOption'
        >
        </quill-editor>
      </section>
    </template>

    <script>
      export default {
        data () {
          return {
            editorContent: '',
            editorOption: {
              placeholder: 'What’s on your mind?',
              theme: 'snow',
              modules: {
                markdownShortcuts: {},
                toolbar: [
                  [{ header: [1, 2, false] }],
                  ['image', 'blockquote']
                ]
              }
            }
          }
        }
      }
    </script>
    ```

# Contributing

Unless they specifically concern this fork and the integration with Vue, Nuxt, and vue-quill-editor, please contribute directly to [Patrick’s original package](https://github.com/patleeman/quill-markdown-shortcuts). I’ll be updating this package with any improvements made upstream.

Otherwise, issues and pull requests specifically concerning this fork are always welcome.
