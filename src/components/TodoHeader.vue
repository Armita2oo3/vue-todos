<script setup>
import { RouterLink } from "vue-router";

import { Icon } from "@iconify/vue";
</script>
<template>
  <header>
    <nav class="container">
      <div class="branding">
        <Icon
          class="logo"
          icon="line-md:list-3-filled"
          color="green"
          width="22"
          height="22"
        />
        <h1>Todo List</h1>
        <ul class="nav-routes">
          <RouterLink to="/">Home </RouterLink>
          <RouterLink to="/about">About </RouterLink>
        </ul>
      </div>
    </nav>
  </header>
</template>

<style lang="scss" scoped>
header {
  background-color: #f1f1f1;
  nav {
    align-items: center;
    padding: 25px 10px;

    .branding {
      display: flex;
      align-items: center;
      gap: 8px;
      .logo {
        max-width: 50px;
      }

      h1 {
        font-size: 24px;
      }
    }

    .nav-routes {
      gap: 12px;
      list-style: none;
      flex: 1;
      justify-content: flex-end;
      display: flex;

      a {
        text-decoration: none;
        color: inherit;
      }
    }
  }
}
</style>
