<template>
  <contentBase>
    <div class="row">
      <div class="col-3">
        <UserProfileInfoView :user="user" @follow="follow" @unfollow="unfollow" />
        <UserProfileWriteView @post_a_post="post_a_post" />
      </div>
      <div class="col-9">
        <UserProfilePostView :posts="posts" />
      </div>
    </div>
  </contentBase>
</template>

<script>
import { reactive } from 'vue';
import ContentBase from "../components/ContentBase.vue";
import UserProfileInfoView from '../components/UserProfileInfoView.vue'
import UserProfileWriteView from '../components/UserProfileWriteView.vue';
import UserProfilePostView from '../components/UserProfilePostView.vue';
import { useRoute } from 'vue-router'

export default {
  name: "UserProfileView",
  components: {
    ContentBase,
    UserProfileInfoView,
    UserProfileWriteView,
    UserProfilePostView,
  },
  setup() {
    const route = useRoute();
    const userId = route.params.userId;
    
    const user = reactive({
      id: 1,
      userName: 'Chubao',
      firstName: 'Bao',
      lastName: 'Chu',
      is_followed: false,
      followerCount: 0,
    });

    const posts =reactive({
      count: 3,
      post: [
        {
          id: 1,
          userId: 1,
          content: "今天是美好的一天"
        },
        {
          id: 2,
          userId: 1,
          content: "今天没有好好学习"
        },
        {
          id: 3,
          userId: 1,
          content: "今天就学了点Vue"
        }
      ]
    })

    const follow = () => {
      if (user.is_followed) return ;
      user.is_followed = true;
      user.followerCount ++ ;
    }

    const unfollow = () => {
      if (!user.is_followed) return ;
      user.is_followed = false;
      user.followerCount -- ;
    }

    const post_a_post = (content) => {
      if (content == "") return ;
      posts.post.unshift({
        id: posts.count,
        userId: 1,
        content: content
      });
      content = '';
    }

    return {
      user,
      follow,
      unfollow,
      posts,
      post_a_post,
      userId
    }
  }
};
</script>

<style scoped>
</style>