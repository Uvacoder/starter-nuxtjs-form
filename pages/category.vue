<template>
  <main class="container">
    <div class="container-fluid section">
      <div class="row">
        <div class="col-12">
          <b-form @submit="onSubmit" v-if="show" style="margin-top: 3rem">
            <h2>Add new category</h2>

            <!--  Category Type Input  -->
            <b-form-group label="Category Type" label-for="category-type">
              <b-form-input
                v-model="type"
                placeholder="Category Type"
                size="sm"
                required
              ></b-form-input>
            </b-form-group>

            <b-button type="submit" @click="onAddCategory" variant="primary"
              >Add Category</b-button
            >
          </b-form>

          <nuxt-link to="/"
            ><b-button class="button mt-2" variant="dark">Go to Posts</b-button></nuxt-link
          >

          <div class="categories mt-2" v-for="category in categories" :key="category._id">
             <b-button pill variant="primary" alt="category. not a button">{{ category.type }}</b-button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get("http://localhost:8000/api/categories");
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      type: "",
      categories: [],
      show: true,
    };
  },
  methods: {
    async onAddCategory() {
      let data = { type: this.type };
      let result = await this.$axios.$post("http://localhost:8000/api/categories", data);
      this.categories.push(data);
    },
    onSubmit(event) {
      event.preventDefault();
    },
  },
};
</script>

<style scoped>
.section {
  margin: 0 auto;
  height: 100vh;
  display: flex;
  justify-content: center;
}
</style>
