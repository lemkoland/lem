<template>
  <Layout>
    <h1>Zobacz z kategorii <br> {{ $page.wordPressCategory.title }} </h1>
    <ul class="post-list">
      <li v-for="{ node } in $page.wordPressCategory.belongsTo.edges" :key="node.id">
        <Post :post="node" />
      </li>
    </ul>
    <Pager :info="$page.wordPressCategory.belongsTo.pageInfo"/>
  </Layout>
</template>

<page-query>
query WordPressCategory ($id: ID!, $page: Int) {
  wordPressCategory(id: $id) {
    title
    belongsTo(page: $page, perPage: 10) @paginate {
      pageInfo {
        totalPages
        currentPage
      }
      edges {
        node {
          ... on WordPressPost {
            id
            title
            path
            excerpt
            tags {
              title
              path
              id
            }
            content
            featuredMedia {
              sourceUrl
              altText
              mediaDetails {
                width
              }
            }
        	}
        }
      }
    }
  }
  allWordPressPage {
    edges {
      node {
        title
        path
      }
    }
  }
  allWordPressPostTag {
    edges {
      node {
        title
        id
        path
      }
    }
  }
  allWordPressCategory (sortBy: "name", order: ASC) {
edges {
  node {
    title
    id
    path
  }
}
}
}
</page-query>

<script>
import { Pager } from 'gridsome'
import Post from '~/components/Post.vue'

export default {
  components: {
    Pager,
    Post
  },
  metaInfo () {
    return {
      title: this.$page.wordPressCategory.title
    }
  }
}
</script>
