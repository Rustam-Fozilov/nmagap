<template>
  <div class="nav-wrapper">
    <div class="nav-title">Mini clone of Twitter</div>
    <div class="nav-items">
      <ul>
        <li><router-link to="/">HOME</router-link></li>
        <li><router-link to="/about">ABOUT</router-link></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "TheNavbar"
}
</script>

<style scoped>
.nav-wrapper {
  height: 100px;
  background-color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 70px;
}

.nav-title {
  font-size: 28px;
  font-family: Montserrat-SemiBold;
  letter-spacing: -1px;
}

.nav-items ul {
  margin: 0;
}

.nav-items ul li {
  display: inline-block;
  margin-left: 20px;
}

.nav-items ul li a {
  font-family: Montserrat-Regular;
  text-decoration: none;
  color: black;
}
</style>