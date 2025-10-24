<template>
  <div class="networkError">
    <h1>Uh-Oh!</h1>

    <h3>
      It looks like you're experiencing some network issues, please take a breath and
      <a href="#" @click="$router.go(-1)">click here</a> to try again.
    </h3>
  </div>
</template>

<style scoped>
.networkError {
  text-align: center;
  padding: 24px;
}
.networkError a {
  color: #42b983;
  text-decoration: none;
}
</style>