<template>
  <div class="posts-page">
    <div class="container-fluid add-post-section">
      <div class="row">
        <div class="col-12">
          <h3>All Posts</h3>
          <nuxt-link to="/products"
            ><b-button class="button" variant="primary">Add New Post</b-button></nuxt-link
          >
          <nuxt-link to="/category"
            ><b-button class="button" variant="light"
              >Add New Category</b-button
            ></nuxt-link
          >
          <nuxt-link to="/owner"
            ><b-button class="creator-button" variant="dark"
              >Add New Creator</b-button
            ></nuxt-link
          >
        </div>
      </div>
    </div>

    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <div class="grid">
            <div v-for="p of products" :key="p._id">
              <div class="card mx-2 my-2" style="width: 18rem">
                <img :src="p.photo" class="card-img-top" alt="..." />
                <div class="card-body">
                  <h5 class="card-title">{{ p.title }}</h5>
                  <p class="card-text">{{ p.description }}</p>
                  <nuxt-link :to="`/products/${p._id}`"
                    ><a class="btn btn-primary">View</a></nuxt-link
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get("http://localhost:8000/api/products");
      console.log(response);
      return {
        products: response.products,
      };
    } catch (err) {
      console.log(err, "loading api failed on nuxt frontend");
    }
  },
  data() {
    return {
      products: [],
    };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto&family=Space+Grotesk&display=swap");

.posts-page {
  min-height: 100vh;
}

.add-post-section {
  margin: 0 auto;
  width: 50%;
  margin-top: 3rem;
  margin-bottom: 2rem;
}

.grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.list {
  max-width: 1640px;
}
.button {
  margin-bottom: 1rem;
}
.creator-button {
  margin-bottom: 1rem;
}
</style>
