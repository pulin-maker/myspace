<template>
<ContentBase>
    <div class="row">
        <div class="col-3 img-field">
            <img class="img-fluid" :src="user.photo" alt="">
        </div>
        <div class="col-9">
            <div class="my_name">{{ user.username }}</div>
            <div class="my_fans">Fans: {{ user.followerCount }}</div>
            <button @click="follow" v-if="!user.is_followed" type="button" class="btn btn-secondary btn-sm">+ 关注</button>
            <button @click="unfollow" v-else type="button" class="btn btn-secondary btn-sm">取消关注</button>
        </div>
    </div>
</ContentBase>
</template>

<script>
import $ from 'jquery'
import ContentBase from '../components/ContentBase.vue'
import { useStore } from 'vuex'

export default {
    name: "UserProfileInfoView",
    components: {
        ContentBase,
    },
    props: {
      user: {
        type: Object,
        required: true,
      }
    },
    setup(props, context) {
        const store = useStore();
        const follow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id
                },
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access
                },
                success(resp) {
                    if (resp.result === "success")
                       context.emit('follow');
                }
            })
        }

        const unfollow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id
                },
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access
                },
                success(resp) {
                  if (resp.result === "success")
                    context.emit('unfollow');
                }
            })
        }

        return {
        follow,
        unfollow
    }
    }

    
}

</script>

<style scoped>
img {
    border-radius: 50%;
}

.my_name {
    font-weight: bold;
    font-size: 18px;
}

.my_fans {
    font-size: 18px;
    color: gray;
    margin-top: 10px;
}

button {
    font-size: 12px;
    margin-top: 10px;
}

.img-field {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
</style>