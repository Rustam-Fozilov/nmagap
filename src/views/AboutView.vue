<template>
  <div class="about-block">
    <div class="title">In this app:</div>
    <div class="items">
      <ul>
        <li class="item">🔥 Options API + Axios</li>
        <li class="item">Using Firebase over REST API</li>
        <li class="item">Handling GET, POST, PATCH requests</li>
      </ul>
      <div class="title">And little things:</div>
      <ul>
        <li class="item">Write a comment on a tweet</li>
        <li class="item">Generating images and colors randomly</li>
      </ul>
    </div>
    <a href="https://github.com/rustam-fozilov" target="_blank">Developer: Rustam Fozilov</a>
  </div>
</template>

<script>
export default {
  name: "AboutView"
}
</script>

<style scoped>
.about-block {
  margin-top: 30px;
}

.title {
  font-family: Montserrat-SemiBold;
  font-size: 28px;
  margin-bottom: 2rem;
}

.item {
  font-family: Montserrat-Regular;
  font-size: 1rem;
  margin-bottom: 1rem;
}

a {
  font-family: Montserrat-Regular;
  opacity: 0.6;
}
</style>