<template>
  <main class="container">
    <b-container fluid>

      <b-col class="section">
        <b-form @submit="onSubmit" v-if="show" style="margin-top: 3rem">
          <h2>Update Post</h2>

          <!-- Category Dropdown -->
          <b-form-group label="Category" label-for="category">
            <b-form-select v-model="categoryID" size="sm" id="category">
              <template #first>
                <b-form-select-option :value="null" disabled
                  >-- Please select an option --</b-form-select-option
                >
              </template>
              <b-form-select-option
                v-for="category in categories"
                :key="category._id"
                :value="category._id"
                >{{ category.type }}</b-form-select-option
              >
            </b-form-select>
          </b-form-group>

          <!-- Owner Dropdown  -->
          <b-form-group label="Owner" label-for="owner">
            <b-form-select
              v-model="ownerID"
              :options="owners"
              size="sm"
              id="owner"
            >
              <template #first>
                <b-form-select-option :value="null" disabled
                  >-- Please select an option --</b-form-select-option
                >
              </template>
              <b-form-select-option
                v-for="owner in owners"
                :key="owner._id"
                :value="owner._id"
                >{{ owner.name }}</b-form-select-option
              >
            </b-form-select>
          </b-form-group>

          <!--  Title  -->
          <b-form-group label="Title" label-for="title">
            <b-form-input
              v-model="title"
              placeholder="Title"
              size="sm"
            ></b-form-input>
          </b-form-group>

          <!-- Description Textarea -->
          <b-form-group
            label="Description"
            label-for="description"
          >
            <b-form-textarea
              v-model="description"
              size="sm"
              placeholder="Description"
            ></b-form-textarea>
          </b-form-group>

          <!--  Photo  -->
          <b-form-group label="Add Photo" label-for="photo">
            <b-form-file
              v-model="selectedFile"
              placeholder="Add a file or drop it here..."
              drop-placeholder="Drop image file here..."
              @change="onFileSelected"
            ></b-form-file>
            <p>{{ fileName }}</p>
          </b-form-group>

          <b-button type="submit" @click="updatePost" variant="primary">Update Post</b-button>
        </b-form>
      </b-col>
    </b-container>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    try {
      const categories = $axios.$get("http://localhost:8000/api/categories");
      const owners = $axios.$get("http://localhost:8000/api/owners");
      const product = $axios.$get(`http://localhost:8000/api/products/${params.id}`)
      const [catResponse, ownerResponse, productResponse] = await Promise.all([categories, owners, product]);
      console.log(productResponse)
      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      // price: '',
      // stockQuantity: '',
      description: "",
      selectedFile: null,
      fileName: "",
      owners: [],
      categories: [],
      show: true,
    };
  },
  methods: {
    onFileSelected(event) {
      event.preventDefault()
      this.selectedFile = event.target.files[0]
      console.log(this.selectedFile)
      this.fileName = event.target.files[0].name 
    },
    async updatePost() {
      let data = new FormData()
      data.append('title', this.title)
      data.append('description', this.description)
      data.append('ownerID', this.ownerID)
      data.append('categoryID', this.categoryID)
      // data.append('price', this.price);
      // data.append('stockQuantity', this.stockQuantity);
      data.append('photo', this.selectedFile, this.selectedFile.name)
      let result = await this.$axios.$put(`http://localhost:8000/api/products/${this.$route.params.id}`, data)
      this.$router.push('/')
    },
    onSubmit(event) {
      event.preventDefault()
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