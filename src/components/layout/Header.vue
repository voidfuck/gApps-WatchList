<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Vue Quiz App</b-nav-item>
      <b-nav-item disabled>{{correctNum}}/{{total}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["correctNum", "total"]
};
</script>