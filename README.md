# vue-clipboard2-next

A simple vuejs 3 binding for clipboard.js

## Install

`npm install --save vue-clipboard2-next` or use `dist/vue-clipboard.min.js` without webpack

## Usage

For vue-cli user:

```javascript
import {createApp} from 'vue'
import VueClipboard from 'vue-clipboard2-next'

const app = createApp()
app.use(VueClipboard)
```

For standalone usage:

```html
<script src="vue.min.js"></script>
<!-- must place this line after vue.js -->
<script src="dist/vue-clipboard.min.js"></script>
```

## I want to copy texts without a specific button!

Yes, you can do it by using our new method: `this.$copyText`. See
[sample2](https://github.com/jiahwa/vue-clipboard2-next/blob/master/samples/sample2.html),
where we replace the clipboard directives with a v-on directive.

Modern browsers have some limitations like that you can't use `window.open` without a user interaction.
So there's the same restriction on copying things! Test it before you use it. Make sure you are not
using this method inside any async method.

Before using this feature, read:
[this issue](https://github.com/zenorocha/clipboard.js/issues/218) and
[this page](https://github.com/zenorocha/clipboard.js/wiki/Known-Limitations) first.

## It doesn't work with bootstrap modals

See [clipboardjs](https://clipboardjs.com/#advanced-usage) document and [this pull request](https://github.com/jiahwa/vue-clipboard2-next/pull/23), `container` option is available like this:

```js
let container = this.$refs.container
this.$copyText("Text to copy", container)
```

Or you can let `vue-clipboard2-next` set `container` to current element by doing this:

```js
import {createApp} from 'vue'
import VueClipboard from 'vue-clipboard2-next'

const app = createApp()
VueClipboard.config.autoSetContainer = true // add this line
app.use(VueClipboard)
```

## Sample

```html
<div id="app"></div>

<template id="t">
  <div class="container">
    <input type="text" v-model="message">
    <button type="button"
      v-clipboard:copy="message"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError">Copy!</button>
  </div>
</template>

<script>
  import {createApp, ref} from 'vue'

  createApp({

    template: '#t',

    setup() {
      const message = ref('Copy These Text')
      const onCopy = e => alert('You just copied: ' + e.text)
      const onError = e => alert('Failed to copy texts')

      return {
        message,
        onCopy,
        onError
      }
    },
  }).mount('#app')
</script>
```

## Sample 2

```html
<div id="app"></div>

  <template id="t">
    <div class="container">
    <input type="text" v-model="message">
    <button type="button" @click="doCopy">Copy!</button>
    </div>
  </template>

  <script>
    import {createApp, ref} from 'vue'

    createApp({

      template: '#t',
      
      setup() {
        const message = ref('Copy These Text')
        const doCopy = () => {
          this.$copyText(message.value).then((e) => {
            alert('Copied')
            console.log(e)
          }, (e) => {
            alert('Can not copy')
            console.log(e)
          })
        }

        return {
          message,
          doCopy
        }
      }
    }).mount('#app')
  </script>

```

You can use [your Vue instance ```vm.$el```](https://vuejs.org/v2/api/#vm-el) to get DOM elements via the usual traversal methods, e.g.:

```this.$el.children[1].children[2].textContent```

This will allow you to access the *rendered* content of your components, rather than the components themselves.

### Contribution

PRs welcome, and issues as well! If you want any feature that we don't have currently,
please fire an issue for a feature request.

### License

[MIT License](https://github.com/jiahwa/vue-clipboard2-next/blob/master/LICENSE)
