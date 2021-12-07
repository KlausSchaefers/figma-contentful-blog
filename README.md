# figma-contentful-blog

This project shows how to use Figma and ContentFul.com to build a blog, using the Luisa Low-Code framework. 
The Figma design was extended with The Figma-Low-Code plugin to wire certain elements to the content full elements.

The Home.vue file contains all the code.


```vue

<template>
  <Luisa :design="design" :config="config" v-model="viewModel"/>
</template>
<script>
/**
 * Here is the downloaded figma file. To download yours, type in the command line
 * node download.js.
 */
import app from './app.json'
import * as contentful from 'contentful';
import { documentToHtmlString } from '@contentful/rich-text-html-renderer';

export default {
  name: 'Home',
  data: function() {
    return {
      design: app,
      viewModel: {
        selected: {},
        blog: [
          {
            id: 1,
            body: {
              type: 'richtext',
              value: '<b>This is bold text</b>'
            },
            header: '',
            summary: 'This is the first blog',
            title: 'First'
          }
        ]
      },
      config: {
      }
    }
  },
  components: {
  },
  methods: {
    loadBlogArticle () {

    },
    async loadContentFul () {
      let entries = await this.client.getEntries()
      this.viewModel.blog = entries.items.map(item => {
        let fields = item.fields
          return {
            id: item.sys.id,
            body: {
              type: 'richtext',
              value: documentToHtmlString(fields.body)
            },
            header: 'https:'+ fields.header?.fields?.file?.url,
            summary: fields.summary,
            title: fields.title
          }
      })
    }
  },
  mounted () {
    this.client = contentful.createClient({
        space: '',
        environment: 'master',
        accessToken: '',
    })
    this.loadContentFul()
  }
}
</script>

```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your unit tests
```
npm run test:unit
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
