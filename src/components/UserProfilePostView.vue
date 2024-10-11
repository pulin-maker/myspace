<template>
  <ContentBase>
    <div class="my-content" v-for="post in posts.post" :key="post.id">
      <div class="card">
        <div class="card-body">
          {{ post.content }}
          <button type="button" @click="delete_a_post(post.id)" class="btn btn-danger btn-sm">删除</button>
        </div>
      </div>
    </div>
  </ContentBase>
</template>

<script>
import $ from 'jquery';
import { computed } from 'vue';
import { useStore } from 'vuex';
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
    user: {
      type: Object,
      required: true
    }
  },
  setup(props, context) {
    const store = useStore();

    let is_me = computed(() => store.state.user.id === props.user.id);

    const delete_a_post = post_id => {
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/post/",
        type: "DELETE",
        data: {
          post_id: post_id
        },
        headers: {
          "Authorization": "Bearer " + store.state.user.access
        },
        success(resp) {
          if (resp.result === "success") 
          context.emit('delete_a_post', post_id)
        }
      })
    }

    return {
      is_me,
      delete_a_post
    }
  }
};
</script>

<style scoped>
.my-content {
    margin-bottom: 10px;
}

button {
  float: right;
  font-size: 16;
}
</style>
