<template>
  <main class="container-profile">
    <div class="container-fluid section">
      <div class="row">
        <div class="col-12">
          <b-form @submit="onSubmit" style="margin-top: 3rem">
            <h2>Your Profile</h2>

            <!--  Name  -->
            <b-form-group label="Name" label-for="name">
              <b-form-input
                v-model="profile"
                :placeholder="$auth.$state.user.name"
                size="sm"
              ></b-form-input>
            </b-form-group>

            <!--  Email  -->
            <b-form-group label="email" label-for="email">
              <b-form-input
                v-model="email"
                :placeholder="$auth.$state.user.email"
                size="sm"
              ></b-form-input>
            </b-form-group>

            <!--  Password  -->
            <b-form-group label="Password" label-for="password">
              <b-form-input
                v-model="password"
                placeholder="password"
                size="sm"
              ></b-form-input>
            </b-form-group>

            <b-button type="submit" @click="updateProfile" variant="primary"
              >Update</b-button
            >
          </b-form>

          <nuxt-link to="/"
            ><b-button class="button mt-2" variant="dark">All Posts</b-button></nuxt-link
          >
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  layout: "default",
  data() {
    return {
      name: "",
      email: "",
      password: ""
    };
  },
  methods: {
    async updateProfile() {
      let data = { name: this.name, email: this.email, password: this.password };

      try {
          let result = await this.$axios.$put("http://localhost:8000/api/auth/user", data);

            if (response.success) {
                this.name = ""
                this.email = ""
                this.password = ""
            }

            await this.$auth.fetchUser()

      } catch (err) {
          console.log(err)
      }
    },
    onSubmit(event) {
      event.preventDefault();
    },
  },
};
</script>

<style>
.container-profile {
  margin: 0 auto;
  display: flex;
  height: 100vh;
  flex-direction: column;
  margin-left: 1rem;
  margin-right: 1rem;
}
.intro-card {
  margin-top: 1rem;
  margin-bottom: 1rem;
}
.form-container {
  margin-top: 1rem;
}
</style>
