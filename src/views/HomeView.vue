<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
  },
  created: function () {
    this.getChecklist()
  },
  methods: {
    getChecklist: function () {
      console.log('GET Checklist Start')
      axios
        .get('http://94.74.86.174:8080/api/checklist', {
          headers: {
            Authorization:
              'Bearer eyJhbGciOiJIUzUxMiJ9.eyJyb2xlcyI6W119.i2OVQdxr08dmIqwP7cWOJk5Ye4fySFUqofl-w6FKbm4EwXTStfm0u-sGhDvDVUqNG8Cc7STtUJlawVAP057Jlg',
          },
        })
        .catch((e) => {
          console.error('FAIL TO FETCH Checklists', e)
        })
        .then((res) => {
          console.log('FETCH RESULT', res)
          console.log('FETCH RESULT DATA', res.data)
          console.log('FETCH RESULT DATA Checklist', res.data.data, res.data.data.length)
        })
        .finally(() => {
          console.log('FETCH COMPLETE')
        })
    },
  },
}
</script>
