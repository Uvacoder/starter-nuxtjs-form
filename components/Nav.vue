<template>
  <b-navbar toggleable type="dark" variant="dark">
    <b-navbar-brand to="/" href="#"
      >rosterbox <span class="nav-icon">🏒</span></b-navbar-brand
    >

    <b-navbar-toggle target="navbar-toggle-collapse">
      <template #default="{ expanded }">
        <b-icon v-if="expanded" icon="chevron-bar-up"></b-icon>
        <b-icon v-else icon="chevron-bar-down"></b-icon>
      </template>
    </b-navbar-toggle>

    <b-collapse id="navbar-toggle-collapse" is-nav>
      <b-navbar-nav class="ml-auto">
        <!--  <b-nav-item v-for="(item, index) in menu" :key="index" :href="item.to">
          {{ item.label }}
        </b-nav-item> -->

        <template v-if="$auth.$state.loggedIn">
            <b-nav-item to="/login">Hello, {{ $auth.$state.user.name }}</b-nav-item>
            <b-nav-item to="/profile">Profile</b-nav-item>
            <b-nav-item to="/owner">Add Owner</b-nav-item>
        </template>

        <template v-else>
          <b-nav-item to="/signup">Signup</b-nav-item>
        </template>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
import {
  BIcon,
  BIconArrowUp,
  BIconArrowDown,
  BIconChevronBarDown,
  BIconChevronBarUp,
} from "bootstrap-vue";

export default {
  components: {
    BIcon,
    BIconChevronBarDown,
    BIconChevronBarUp,
  },
  props: {
    // ...
  },
  data() {
    return {
      menu: [
        { to: "#about", label: "About" },
        { to: "#register", label: "Signup" },
        { to: "#login", label: "Login" },
      ],
    };
  },
};
</script>

<style>
#nav {
  margin-bottom: 2rem;
}
.nav-icon {
  margin-left: 1rem;
}
</style>
