<template>
  <ContentBase>
    <div class="my-content" v-for="post in posts.post" :key="post.id">
      <div class="card">
        <div class="card-body">
          {{ post.content }}
        </div>
      </div>
    </div>
  </ContentBase>
</template>

<script>
import ContentBase from "../components/ContentBase.vue";

export default {
  name: "UserProfilePostView",
  components: {
    ContentBase,
  },
  props: {
    posts: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.my-content {
    margin-bottom: 10px;
}
</style>
