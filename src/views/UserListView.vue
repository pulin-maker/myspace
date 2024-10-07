<template>
  <contentBase>
    <div class="card" v-for="user in users" :key="user.id">
      <div class="card-body">
        <div class="row">
          <div class="col-1">
            <img class="img-fluid" :src="user.photo" alt="">
          </div>
          <div class="col-11">
            <div class="username">{{ user.username }}</div>
            <div class="fans">Fans: {{ user.followerCount }}</div>
          </div>
        </div>
      </div>
    </div>
  </contentBase>
</template>

<script>
import ContentBase from "../components/ContentBase.vue";
import { ref } from 'vue';
import $ from 'jquery'

export default {
  name: "UserListView",
  components: {
    ContentBase,
  },
  setup() {
    let users = ref([]);

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/userlist/",
      type: "get",
      success(resp) {
        users.value = resp;
      }
    })

    return {
      users
    }
  }
};
</script>

<style scoped>
img {
  border-radius: 50%;
}

.username {
  font-weight: bold;
  font-size: 18px;
}

.fans {
  color: gray;
  font-size: 16px;
  margin-top: 10px;
}

.card {
  margin-bottom: 16px;
  cursor: pointer;
}

.card:hover {
  box-shadow: 2px 2px 10px lightgray;
  transition: 500ms;
}
</style>