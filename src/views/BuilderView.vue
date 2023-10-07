<template>
  <div id="app">
    <div>
      <div v-if="canShowContent">

        <builder-render-content
          model="page"
          :content="content"
          :api-key="apiKey"
          :customComponents="getRegisteredComponents()"
        />
      </div>
      <div v-else>Content not Found</div>
    </div>
  </div>
</template>

<script>
import { RenderContent, getContent, isPreviewing } from '@builder.io/sdk-vue/vue2';
import '@builder.io/sdk-vue/vue2/css';

// Register your Builder components
import HelloWorldComponent from '../components/HelloWorld.vue';

export const REGISTERED_COMPONENTS = [
  {
    component: HelloWorldComponent,
    name: 'Hello World',
    canHaveChildren: true,
    inputs: [
      {
        name: 'text',
        type: 'string',
        defaultValue: 'World',
      },
    ],
  },
];

// TODO: enter your public API key
const BUILDER_PUBLIC_API_KEY = '144a4a1d475e4db5a564a8da799d2bfc'; // ggignore

export default {
  name: 'App',
  components: {
    'builder-render-content': RenderContent,
  },
  data: () => ({
    canShowContent: false,
    content: null,
    apiKey: BUILDER_PUBLIC_API_KEY,
  }),
  methods: {
    getRegisteredComponents() {
      return REGISTERED_COMPONENTS;
    },
  },
  mounted() {
    getContent({
      model: 'page',
      apiKey: BUILDER_PUBLIC_API_KEY,
      userAttributes: {
        urlPath: window.location.pathname,
      },
    }).then(res => {
      this.content = res;
      this.canShowContent = this.content || isPreviewing();
    });
  },
};
</script>

<style>


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100vw;
}
</style>
