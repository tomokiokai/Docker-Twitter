<template>
  <div class="flex">
    <div class="left">
      <SideNav @shared="addPost" />
    </div>
    <div class="right">
      <div class="title">
        <p>ホーム</p>
      </div>
      <div v-for="post in posts" :key="post.id">
        <Message :post="post" :uid="uid" @like="like(post)" @unlike="unlike(post)" @deletePost="deletePost" />
      </div>
    </div>
  </div>
</template>

<script>

export default {
  middleware: "auth",
  data() {
    return {
      posts: [],
      uid: null,
    };
  },
  methods: {
    async getPostData() {
      const { data } = await this.$axios.get(
        "http://127.0.0.1:8000/api/post"
      );
      this.posts = data.posts;
    },

    fetchData() {
      async (user) => {
        this.uid = user.uid;
        this.getPostData();
      };
    },

    async like(post) {
      const { data } = await this.$axios.post(
        "http://localhost:8000/api/like",
        {
          post_id: post.id,
          user_id: this.uid,
        }
      );
      post.likes.push(data.like);
    },
    async unlike(post) {
      const findLike = post.likes.find((like) => like.user_id === this.uid);
      await this.$axios.delete(
        `http://localhost:8000/api/like/${findLike.id}`
      );

      const findLikeIdx = post.likes.findIndex(
        (like) => like.id === findLike.id
      );
      post.likes.splice(findLikeIdx, 1);
    },
    async deletePost(event) {
      await this.$axios.delete(`http://localhost:8000/api/post/${event.id}`);

      const findPostIdx = this.posts.findIndex((post) => post.id === event.id);
      this.posts.splice(findPostIdx, 1);
    },
    addPost(post) {
      this.posts.push(post);
    },
  },
  created() {
    this.fetchData();
  },

};
</script>

<style scoped>
.left {
  width: 22%;
  height: 100vh;
}

.right {
  width: 78%;
  height: 100vh;
}

.flex {
  display: flex;
}

.title {
  border-bottom: 1px solid white;
  border-left: 1px solid white;
  padding: 15px;
}

.title p {
  font-size: 20px;
  font-weight: bold;
}
</style>