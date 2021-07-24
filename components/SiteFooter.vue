<template>
  <footer style="box-sizing: unset" class="site-footer">
    {{ $siteConfig.siteName }} &copy; {{ new Date().getFullYear() }} - All
    Rights Reserved
    <br />
  </footer>
</template>

<script>
export default {
  name: 'SiteFooter'
}
</script>

<style scoped>
footer {
  box-sizing: none !important;
  background: #eee;
  padding: 20px 10px;
  text-align: center;
  transition: 0.5s ease all;
}
</style>
