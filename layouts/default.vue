<template>
  <div class="containerAll">
    <appHeader></appHeader>
    <nuxt />
    <appFooter></appFooter>
  </div>
</template>
<script>
import Header from '@/components/Header';
import Footer from '@/components/Footer';

export default {
  components: {
    appHeader: Header,
    appFooter: Footer
  }
}

</script>
