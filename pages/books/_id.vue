<template>
  <div>
    <h2>{{ title }}</h2>
    <ul v-if="sentences">
      <li v-for="sentence in sentences" :key="sentence.id">
        {{ sentence.text }}
      </li>
    </ul>
    <div>
      <div @click="moreRead">
        続きを読む
      </div>
      <div>取り消す</div>
      <div>最初から読む</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    title: null,
    sentences: [],
    page: 1
  }),
  async mounted() {
    const res = await axios.get(
      `http://bungo-chat-api.herokuapp.com/api/books/${this.$nuxt.$route.params.id}`
    )
    this.title = res.data.title
  },
  methods: {
    async moreRead() {
      const res = await axios.get(
        `http://bungo-chat-api.herokuapp.com/api/books/${this.$nuxt.$route.params.id}?`,
        {
          params: { page: this.page, amount: 0 }
        }
      )
      this.sentences.push(...res.data.sentences)
      this.page++
    }
  }
}
</script>
