<template>
  <div>
    <Header></Header>
    <Body></Body>
    <Footer></Footer>
  </div>
</template>

<script>
import Header from "./component/Header.vue";
import Body from './component/Body.vue';
import Footer from "./component/Footer.vue";

export default {
  data: function() {
    return {
      
    };
  },
  components: {
    Header: Header,
    Body: Body,
    Footer: Footer
  },
  // beforeCreate: function(){
  //   console.log('before created')
  // },
  // created: function(){
  //   console.log("created")
  // },
  // beforeMounted: function(){
  //   console.log('vefore mounted')
  // },
  // mounted: function(){
  //   console.log('mounted')
  // },
  // beforeUpdate: function(){
  //   console.log('before update')
  // },
  // updated: function(){
  //   console.log('update')
  // }
};
</script>


<style scoped>
  
</style>
