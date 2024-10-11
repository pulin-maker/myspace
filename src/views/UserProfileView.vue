<template>
  <contentBase>
    <div class="row">
      <div class="col-3">
        <UserProfileInfoView :user="user" @follow="follow" @unfollow="unfollow" />
        <UserProfileWriteView v-if="is_me" @post_a_post="post_a_post" />
      </div>
      <div class="col-9">
        <UserProfilePostView :user="user" :posts="posts" @delete_a_post="delete_a_post" />
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
import $ from 'jquery';
import { useStore } from 'vuex'
import { computed } from 'vue'

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
    const userId = parseInt(route.params.userId);
    const store = useStore();
    
    const user = reactive({});
    const posts = reactive({});

    $.ajax({
      url: 'https://app165.acapp.acwing.com.cn/myspace/getinfo/',
      type: 'get',
      data: {
        user_id: userId,
      },
      headers: {
        'Authorization': 'Bearer ' + store.state.user.access,
      },
      success(resp) {
        user.id = resp.id;
        user.username = resp.username;
        user.followerCount = resp.followerCount;
        user.photo = resp.photo;
        user.is_login = resp.is_login;
        user.is_followed = resp.is_followed;
      }
    });

    $.ajax({
      url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
      type: 'get',
      data: {
        user_id: userId
      },
      headers: {
        'Authorization': 'Bearer ' + store.state.user.access
      },
      success(resp) {
        posts.post = resp;
        posts.count = resp.length;
      }
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
      posts.post.unshift({
        id: posts.count,
        userId: store.state.user.id,
        content: content
      });
      content = '';
    }

    const delete_a_post = post_id => {
      posts.post = posts.post.filter(post => post.id !== post_id);
      posts.count = posts.post.length;
    }

    /* 完整的，有大括号和返回值return
    const is_me = computed(() => {
        return userId === store.state.user.user_id
    }); 
    */

    // 直接省略大括号和return函数  两种写法是等价的
    const is_me = computed(() => userId === store.state.user.id)

    return {
      user,
      follow,
      unfollow,
      posts,
      post_a_post,
      userId,
      is_me,
      delete_a_post
    }
  }
};
</script>

<style scoped>
</style>