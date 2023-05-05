<template>
    <form @submit.prevent>
        <h4>Create a post</h4>
        <my-input v-model="post.title" required type="text" placeholder="Title" />
        <my-input v-model="post.body" required type="text" placeholder="Content" />
        <my-button class="btn" style="align-self: flex-end; margin-top: 15px;" :disabled="!post.title || !post.body"
            @click="createPost">Create a post</my-button>
    </form>
</template>

<script>

export default {
    data() {
        return {
            post: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit('create', this.post);
            this.post = {
                title: '',
                body: ''
            }
        }
    }
}
</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
}
</style>