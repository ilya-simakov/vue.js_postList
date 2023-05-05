<template>
    <div class="app">
        <h1>List of posts</h1>
        <my-input v-model="searchQuery" placeholder="Search..." />
        <div class="app__btns">
            <my-button @click="showDialog">Create a post</my-button>
            <my-select v-model="selectedSort" :options="sortOptions" />
        </div>

        <my-dialog v-model:show="dialogVisible">
            <post-form @create="createPost" />
        </my-dialog>
        <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostLoading" />
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyButton from "@/components/UI/MyButton";
import MySelect from "@/components/UI/MySelect";
import axios from 'axios';

export default {
    components: {
        MySelect,
        MyButton,
        PostList, PostForm
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostLoading: false,
            selectedSort: '',
            searchQuery: '',
            sortOptions: [
                { value: 'title', name: 'By title' },
                { value: 'body', name: 'By content' },
            ]
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false; // hide dialog window
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible = true;
        },
        async fetchPosts() {
            try {
                this.isPostLoading = true;
                setTimeout(async () => {
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=5');
                    this.posts = response.data;
                    this.isPostLoading = false;
                }, 1000);
            } catch (e) {
                alert('Ошибка')
            } finally {
                // this.isPostLoading = false; - when no setTimeout!
            }
        }
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    // watch: {
    //     selectedSort(newValue) {
    //         this.posts.sort((post1, post2) => {
    //             return post1[newValue]?.localeCompare(post2[newValue])
    //         })
    //     },
    // }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}

.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;

}
</style>