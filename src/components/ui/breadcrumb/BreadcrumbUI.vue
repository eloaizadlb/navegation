<template>
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li v-for="(crumb, index) in crumbs" :key="index">
          <router-link :to="crumb.to">{{ crumb.label }}</router-link>
          <span v-if="index < crumbs.length - 1"> > </span>
        </li>
      </ol>
    </nav>
  </template>
  
  <script>
  export default {
    computed: {
      crumbs() {
        const segments = this.$route.path.split('/').filter(segment => segment);
        let pathPrefix = '';
  
        return segments.map((segment, index) => {
          pathPrefix += `/${segment}`;
          const label = this.getRouteLabel(segment);
          const to = pathPrefix;
          return { label, to };
        });
      },
    },
    methods: {
      getRouteLabel(segment) {
        return segment.charAt(0).toUpperCase() + segment.slice(1);
      },
    },
  };
  </script>
  
  <style scoped>
  .breadcrumb {
    display: flex;
    list-style: none;
    padding: 0;
  }
  
  .breadcrumb li {
    display: flex;
    align-items: center;
  }
  
  .breadcrumb li:not(:last-child) {
    margin-right: 0.5rem;
  }
  
  .breadcrumb li span {
    margin: 0 0.5rem;
  }
  </style>
  