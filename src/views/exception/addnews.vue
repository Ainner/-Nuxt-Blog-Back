<template>
  <div class="main">
    <a-row :gutter="16">
      <a-col v-for="(item, index) in postList" :key="index" :span="6">
        <a-card :title="item.title" :bordered="false">
          <p>{{ item.content }}</p>
        </a-card>
      </a-col>
    </a-row>
    <a-form class="ant-advanced-search-form" :form="form" @submit="add">
        <a-form-item>
            <a-input v-model="postInfo.title" placeholder="post title" />
        </a-form-item>
        <a-form-item>
            <a-input v-model="postInfo.author" placeholder="post author" />
        </a-form-item>
        <a-form-item>
            <a-textarea
            v-model="postInfo.content[0]"
            placeholder="post content"
            :auto-size="{ minRows: 10, maxRows: 100 }"
            />
        </a-form-item>
        <a-button type="primary" html-type="submit">
        发布
        </a-button>
    </a-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
        postInfo: {
            content: [''],
            title: '',
            author: ''
        },
        postList: [],
        form: this.$form.createForm(this, { name: 'advanced_search' })
    }
  },
  methods: {
      addNews (data) {
            fetch('http://localhost:3000/addArticle/', {
                headers: {
                    'content-type': 'application/json'
                },
                mode: 'cors',
                method: 'POST',
                body: JSON.stringify(data)
            })
            .then(response => {
                this.form.resetFields()
                this.getPostList()
            })
            .catch(error => console.error('Error:', error))
      },
      add () {
          this.addNews(this.postInfo)
      },
      getPostList () {
        fetch('http://localhost:3000/getArticleList/', {
            headers: {
                'content-type': 'application/json'
            },
            method: 'GET'
        })
        .then(res => res.json())
        .then(res => { this.postList = res.data.list })
        .catch(error => console.error('Error:', error))
      }
  },
  mounted () {
      this.getPostList()
  }
}
</script>
