<template>
  <div>
    <Header
      @formDataOutput="(submitData) => formDataOutput(submitData)"
    />
    <pre style="padding: 15px">
    {{ queryData }}
  </pre>
  </div>
</template>

<script>
  import Header from '@/components/Header.vue';

  export default {
    components: {
      Header
    },
    data: () => ({
      queryData: ''
    }),
    methods: {
      formDataOutput(submitData) {
        this.queryData = submitData
      }
    }
  }
</script>

<style lang="scss" src="@/assets/scss/main.scss"></style>
