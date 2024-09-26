<template>
  <ContentBase>
    编辑区：
    <div class="form-floating">
      <textarea v-model="content"
        class="form-control"
        placeholder="Leave a comment here"
        id="floatingTextarea2"
        style="height: 100px"
      ></textarea>
      <label for="floatingTextarea2">请输入：</label>
      <button @click="post_a_post" type="submit" class="btn btn-dark btn=sm">
        提交
      </button>
    </div>
  </ContentBase>
</template>

<script>
import { ref } from "vue";
import ContentBase from "./ContentBase.vue";

export default {
  name: "UserProfileWriteView",
  components: {
    ContentBase,
  },

  setup(props, context) {
    const content = ref('');

    const post_a_post = () => {
        context.emit('post_a_post', content.value);
    }

    return {
      content,
      post_a_post
    };
  },
};
</script>

<style scoped>
button {
  margin-top: 10px;
  font-size: 16px;
}
</style>