<template>
  <div class="d-flex justify-center flex-wrap" style="width:100%">
    <div v-for="n in amount" :key="n">
      <v-skeleton-loader height="150px" width="350px" type="article, actions" class="d-inline-block ma-5"></v-skeleton-loader>
    </div>
  </div>
</template>
<script>
export default {
  data: () => ({}),
  props: {
    amount: Number
  }
};
</script>
