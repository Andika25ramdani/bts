<template>
  <div
    class="p-4 rounded-xl bg-gray-100 h-full mx-auto max-w-4xl flex flex-col gap-8"
  >
    <div class="flex gap-4">
      <input v-model="newCheklistName" type="text" />
      <button @click="addNewChecklist">tambah</button>
    </div>

    <ul class="flex flex-col gap-2 h-full">
      <li
        v-for="item in checklistData"
        :key="item.id"
        class="bg-white py-1 px-4 rounded-lg text-left text-gray-800 flex justify-between"
      >
        <span>{{ item.name }}</span>
        <button @click="deleteChecklistItem(item)" class="text-red-700">
          hapus
        </button>
      </li>
    </ul>
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
  data: function () {
    return {
      newCheklistName: '',
      checklistData: [],
    }
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
          console.log(
            'FETCH RESULT DATA Checklist',
            res.data.data,
            res.data.data.length
          )
          this.checklistData = res.data.data
          console.log('Checklist data', this.checklistData)
        })
        .finally(() => {
          console.log('FETCH COMPLETE')
        })
    },
    addNewChecklist: function () {
      console.log('Post New Checklist Start', this.newCheklistName)
      axios
        .post(
          `http://94.74.86.174:8080/api/checklist`,
          {
            name: this.newCheklistName,
          },
          {
            headers: {
              Authorization:
                'Bearer eyJhbGciOiJIUzUxMiJ9.eyJyb2xlcyI6W119.i2OVQdxr08dmIqwP7cWOJk5Ye4fySFUqofl-w6FKbm4EwXTStfm0u-sGhDvDVUqNG8Cc7STtUJlawVAP057Jlg',
            },
          }
        )
        .catch((e) => {
          console.error('FAIL TO POST New Checklists', e)
        })
        .then((res) => {
          console.log('POST New RESULT', res)
          console.log('POST New RESULT DATA', res.data)
          this.checklistData.push(res.data.data)
        })
        .finally(() => {
          console.log('POST New COMPLETE')
          this.newCheklistName = ''
        })
    },
    deleteChecklistItem: function (checklist) {
      console.log('DELETE Checklist Start')
      axios
        .delete(`http://94.74.86.174:8080/api/checklist/${checklist.id}`, {
          headers: {
            Authorization:
              'Bearer eyJhbGciOiJIUzUxMiJ9.eyJyb2xlcyI6W119.i2OVQdxr08dmIqwP7cWOJk5Ye4fySFUqofl-w6FKbm4EwXTStfm0u-sGhDvDVUqNG8Cc7STtUJlawVAP057Jlg',
          },
        })
        .catch((e) => {
          console.error('FAIL TO DELETE Checklists', e)
        })
        .then((res) => {
          console.log('DELETE RESULT', res)
          console.log('DELETE RESULT DATA', res.data)

          this.checklistData = this.checklistData.filter(
            (item) => item != checklist
          )
        })
        .finally(() => {
          console.log('DELETE COMPLETE')
        })
    },
  },
}
</script>
