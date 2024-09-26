<template>
<ContentBase>
    <div class="row">
        <div class="col-3">
            <img class="img-fluid" src="../assets/logo.jpg" alt="">
        </div>
        <div class="col-9">
            <div class="my_name">{{ fullName }}</div>
            <div class="my_fans">Fans: {{ user.followerCount }}</div>
            <button @click="follow" v-if="!user.is_followed" type="button" class="btn btn-secondary btn-sm">+ 关注</button>
            <button @click="unfollow" v-else type="button" class="btn btn-secondary btn-sm">取消关注</button>
        </div>
    </div>
</ContentBase>
</template>

<script>
import { computed } from 'vue'
import ContentBase from '../components/ContentBase.vue'

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
        let fullName = computed(() => props.user.lastName + ' ' + props.user.firstName);

        const follow = () => {
            context.emit('follow');
        }

        const unfollow = () => {
            context.emit('unfollow');
        }

        return {
        fullName,
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
</style>