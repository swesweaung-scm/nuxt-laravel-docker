<template>
  <div>{{ data.name }}</div>
</template>

<script>
export default {
  name: 'IndexPage',
  async asyncData({ $axios }) {
    const data = await $axios.$get(`/api/v1/test`)
    console.log("data",data)
    return { data }
  },
}
</script>
