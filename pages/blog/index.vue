<template>
  <div v-if="posts" class="container">
    <div id="content" class="site-content">
      <div id="primary" class="content-area column two-thirds">
        <main id="main" class="site-main" role="main">
          <div class="grid bloggrid">
            <article v-for="post in posts">
              <NuxtLink class="link" :to="`/blog/${post.id}`">
                <header class="entry-header">
                  <h1 class="entry-title">{{post.title}}</h1>
                  <div class="entry-meta">
                    <span class="posted-on"><time class="entry-date published">{{createdAtDate(post.createdAt)}}</time></span>
                  </div>
                  <div class="entry-thumbnail">
                    <img :src="post.image" alt="preview-image">
                  </div>
                </header>
                <div class="entry-summary">
                  <p>
                    {{post.preview}}
                  </p>
                </div>
              </NuxtLink>
            </article>
          </div>
          <div class="clearfix">
          </div>
          <nav class="pagination">
            <ul>
              <li @click="prevPage"><</li>
              <li
                v-for="item in 9"
                :key="item"
                @click="setPage(item)"
                :class="{active: pageCount === item}"
              >
                {{item}}
              </li>
              <li @click="nextPage">></li>
            </ul>
          </nav>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
const api = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'
export default {
  async asyncData({query}) {
    const response = await fetch(
      `${api}?&page=${query?.page ? query.page : '1'}&limit=${query.limit ? query?.limit : '10'}`)
    const posts = await response.json()
    return { posts }
  },
  data() {
    return {
      pageCount: +this.$route.query.page || 1,
      limit: 10
    }
  },
  methods: {
    createdAtDate(dateIso) {
      const date = new Date(dateIso)
      return date.toLocaleString('en-us', {
        month: 'long',
        day: 'numeric',
        year: 'numeric'
      })
    },
    setPage(page) {
      this.pageCount = page
      this.$router.push({name: 'blog', query: {page: this.pageCount, limit: this.limit}})
    },
    nextPage() {
      if(this.pageCount >= 9) return
      this.pageCount++
      this.$router.push({name: 'blog', query: {page: this.pageCount, limit: this.limit}})
    },
    prevPage() {
      if(this.pageCount <= 1) return
      this.pageCount--
      this.$router.push({name: 'blog', query: {page: this.pageCount, limit: this.limit}})
    }
  },
  watch: {
    async $route(val) {
      console.log('val', val)
      const response = await fetch(`${api}?&page=${val.query.page}&limit=${val.query.limit}`)
      this.posts = await response.json()
    }
  }
}
</script>

<style scoped>
.container {
  margin:0 20px;
}

.column {
  margin-bottom:1em;
}
.grid .entry-thumbnail img {
  width:100%;
  transition:all 0.4s;
}
.grid .entry-thumbnail img:hover {
  opacity:0.8;
  transition:all 0.4s;
}
article {
  width: 320px;
  border: 1px solid;
  padding: 10px;
}
.grid {
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
}

.link {
  color: unset;
  text-decoration: unset;
}

.bloggrid article.hentry,.single-portfolio .hentry {
  margin-bottom:0;
}
.bloggrid .entry-footer {
  padding-top:0;
}

ul,li {
  padding: 0;
  list-style-type: none;
}

.pagination ul {
  display: flex;
  align-items: center;
  gap: 10px;
}

.pagination li {
  border: 1px solid;
  padding: 10px;
  cursor: pointer;
}

.pagination li.active {
  background: #222222;
  color: #fff;
}

.entry-title {
  font-weight:700;
  font-size:20px;
  letter-spacing:2px;
  line-height:1.4;
  margin-top:0;
  margin-bottom:0;
  text-transform:uppercase;
}
.entry-title a {
  text-decoration:none;
  color:#222;
}
.entry-title a:hover {
  color: #ccc !important;
}
.entry-title { font-family: 'Oswald'; }
.entry-meta {
  font-size:13px;
  font-style:italic;
  font-weight:300;
  letter-spacing:1px;
}
.entry-meta a {
  color:#7f7f7f !important;
  text-decoration:none;
}
.entry-meta a:hover {
  text-decoration:underline;
}
.entry-meta .comments-link:before {
  content:" - ";
}

.entry-meta {
  margin-top: 5px;
}

@media (max-width:600px) {
  .entry-meta {
    font-size:12px !important;
  }
}

.entry-meta,
.cat-links,
.comments-link {
  font-size: 13px;
  font-style: italic;
  font-weight: 300;
  letter-spacing: 0px;
  text-align: left;
  text-transform: none;
}
.entry-thumbnail {
  padding:1em 0 0em 0;
}
.single-portfolio .entry-thumbnail {
  padding: 2em 0 1.5em 0;
}

.page-template-template-portfolio .grid .entry-thumbnail img {
  width:100%;
  transition:all 0.4s;
}
.page-template-template-portfolio .grid .entry-thumbnail img:hover {
  opacity:0.8;
  transition:all 0.4s;
}
.portfoliogrid .entry-thumbnail,.single-portfolio .entry-thumbnail {
  padding-top:0;
}
.page-content,.entry-content,.entry-summary {
  margin:1em 0;
}
.hentry,.site-content .entry-header,.site-content .entry-content,.site-content .entry-summary,.site-content .entry-meta,.page-content,.archive-header,.page-header {
  max-width:100%;
}
.entry-content a,.entry-summary a,.page-content a,.comment-content a {
  text-decoration:none;
}
.bloggrid .entry-footer {
  padding-top:0;
}
.entry-footer {
  font-style:italic;
  padding-top:1em;
  padding-bottom:0.5em;
}
.single-portfolio .entry-footer {
  padding:0;
}
.entry-footer a {
  color:#7f7f7f !important;
  text-decoration:none;
}
.entry-footer a:hover {
  text-decoration:underline;
}
.blog .entry-footer,.search-results .entry-footer,.archive .entry-footer {
  padding-bottom:2em;
}
.page .entry-footer {
  border-bottom:0;
}
.bloggrid .entry-footer {
  padding-top:0;
}
</style>
