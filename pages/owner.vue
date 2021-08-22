<template>
    <div class="container-fluid section">
      <div class="row">
        <div class="col-12">
          <b-form class="mt-4">
            <h2>Add new creator</h2>

            <!--  Name Input  -->
            <b-form-group label="Creator" label-for="owner">
              <b-form-input
                v-model="name"
                placeholder="Post Creator"
                size="sm"
                required
              ></b-form-input>
            </b-form-group>

            <!-- About Textarea -->
            <b-form-group label="About" label-for="about">
              <b-form-textarea
                v-model="about"
                size="sm"
                placeholder="About Me"
              ></b-form-textarea>
            </b-form-group>

            <!-- Image -->
            <b-form-group label="Add Photo" label-for="photo">
              <b-form-file
                v-model="selectedFile"
                placeholder="Add a file or drop it here..."
                drop-placeholder="Drop image file here..."
                @change="onFileSelected"
              ></b-form-file>
              <p>{{ fileName }}</p>
            </b-form-group>

            <b-button type="submit" @click="onAddCreator" variant="primary"
              >Add Creator</b-button
            >
          </b-form>

          <nuxt-link to="/"
            ><b-button class="button mt-2 mb-6" variant="dark"
              >Go to Posts</b-button
            ></nuxt-link
          >
          <div class="grid">
          <div class="owners mt-2" v-for="owner in owners" :key="owner._id">
            <b-button pill variant="primary" class="mr-2 mb-2" alt="category. not a button">{{
              owner.name
            }}</b-button>
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
      let response = await $axios.$get("http://localhost:8000/api/owners");
      return {
        owners: response.owners,
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      name: "",
      about: "",
      owners: [],
      selectedFile: null,
      fileName: "",
      show: true,
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);
      this.fileName = event.target.files[0].name;
    },
    async onAddCreator() {
      let data = new FormData();
      data.append("name", this.name);
      data.append("about", this.about);
      data.append("photo", this.selectedFile, this.selectedFile.name);
      let result = await this.$axios.$post("http://localhost:8000/api/owners", data);
      this.owners.push(this.name);
      console.log(this.owners);
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
