<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-input
      v-model="searchQuery"

    />
    <div class="app__btns">
      <my-button
          @click="showDialog"
      >
        Создать пост
      </my-button>
      <my-select
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>

    <my-dialog v-model:show="dialogVisible" >
      <post-form
          @create="CreatePost"
      />
    </my-dialog>

    <post-list
        :posts="posts"
        @remove="removePost"
        v-if="isPostLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>

</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyDialog from "@/components/UI/MyDialog";
import MyButton from "@/components/UI/MyButton";
import axios from 'axios';
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";
export default {
  components: {
    MyInput,
    MySelect,
    MyButton,
    MyDialog,
    PostList,PostForm
  },
  name: "App.vue",
  data(){
    return{
      posts: [],
      searchQuery: '',
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'}
      ]
    }
  },
  methods:{
    CreatePost(post){
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostLoading = false;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
        this.posts = response.data;
      } catch (e) {
        alert('Error')
      } finally {
        this.isPostLoading = true;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app__btns{
  display: flex;
  justify-content: space-between;
  padding: 15px;
}

.app{
  padding: 20px
}
</style>