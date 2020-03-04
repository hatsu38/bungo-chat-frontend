<template>
  <div>
    <v-app-bar app>
      <v-btn icon>
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <nuxt-link to="/" dark>
        <v-toolbar-title v-text="title" />
      </nuxt-link>
    </v-app-bar>
    <v-content>
      <v-container>
        <v-list v-if="sentences" subheader>
          <v-list-item v-for="sentence in sentences" :key="sentence.id">
            <v-list-item-avatar>
              <v-img v-if="image" :src="image" />
            </v-list-item-avatar>
            <div class="message_body">
              {{ sentence.text }}
            </div>
          </v-list-item>
        </v-list>
      </v-container>
    </v-content>
    <v-footer class="pa-0" app>
      <v-row no-gutters>
        <v-col cols="6" class="border outline-center" @click="moreRead">
          <div class="text-center">
            <v-btn class="ma-1" fab dark>
              <v-icon dark>
                mdi-plus
              </v-icon>
            </v-btn>
            <div>続きを読む</div>
          </div>
        </v-col>
        <v-col cols="6" class="border pa-1 text-center">
          <div>
            <v-icon>fas fa-caret-up</v-icon>
            <div>1つ前に戻る</div>
          </div>
          <div>
            <v-icon>mdi-refresh</v-icon>
            <div>最初から読む</div>
          </div>
        </v-col>
      </v-row>
    </v-footer>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  layout: 'chat',
  data: () => ({
    title: null,
    image: null,
    sentences: [],
    page: 1
  }),
  async beforeMount() {
    const res = await axios.get(
      `https://bungo-chat-api.herokuapp.com/api/books/${this.$nuxt.$route.params.id}`
    )
    this.title = res.data.title
  },
  methods: {
    async moreRead() {
      const res = await axios.get(
        `https://bungo-chat-api.herokuapp.com/api/books/${this.$nuxt.$route.params.id}?`,
        {
          params: { page: this.page, amount: 0 }
        }
      )
      this.sentences.push(...res.data.sentences)
      this.image = res.data.image
      this.page++
    }
  }
}
</script>
<style scoped>
a {
  text-decoration: none;
}
.message_body {
  border: 1px solid #eee;
  border-radius: 5px;
  padding: 8px 12px;
  margin: 5px 0;
  position: relative;
  background: #eee;
}
.message_body::before {
  border: 8px solid transparent;
  display: block;
  position: absolute;
  width: 0;
  height: 0;
  content: '';
  border-right-color: #eee;
  border-left: 0;
  top: 6px;
  left: -9px;
}
.border {
  border: 1px solid #ddd;
}
.outline-center {
  align-items: center;
  display: flex;
  justify-content: center;
}
</style>
