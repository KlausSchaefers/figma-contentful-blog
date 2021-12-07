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
        environment: 'master', // defaults to 'master' if not set
        accessToken: '',
    })
    this.loadContentFul()
  }
}
</script>