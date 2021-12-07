# figma-contentful-blog

This project shows how to use Figma and ContentFul.com to build a blog, using the Luisa Low-Code framework. 


## How does it work

- The blog was designed in Figma and extended with [Figma-Low-Code plugin](https://www.figma.com/community/plugin/858477504263032980/Figma-Low-Code) to wire text elements to the blog data.
- The design is loaded through the Figma API and rendered by the <luisa> component.
- The blog entries are loaded from Contentful.com
- In the Home.vue, the Contentful data is mapped to the viewModel that is passed into the <luisa> component.

The Home.vue file contains all the code. Please note that there is not much front-end code. The entire HTML and CSS is created behind he scenes.


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


## Documentation

You can find the full Luisa documentation at [Luisa.cloud](https://luisa.cloud/help.html).

## Figma-Low-Code Plugin:

[Figma-Low-Code plugin](https://www.figma.com/community/plugin/858477504263032980/Figma-Low-Code)


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
