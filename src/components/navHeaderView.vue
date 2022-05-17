<template>
  <div class="nav">
    <div class="contenaire">
      <router-link to="/"
        ><img
          class="logo"
          src="../assets/icon-left-font-monochrome-white.svg"
          alt="logo"
        />
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "navHeader",
  props: { isActive: Boolean },
};
</script>
<style scoped>
.nav {
  width: 100%;
  height: 60px;
  padding: 10px;
  background-color: rgb(44, 62, 80);
  align-content: center;
}
.logo {
  width: 200px;
}
svg {
  fill: rgb(255, 255, 255);
}
.contenaire {
  width: 90%;
  height: inherit;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.right {
  width: 100px;
  display: flex;
  justify-content: space-around;
}
</style>
