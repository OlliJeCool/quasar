
<template>
  
  <button @click="getLists()">Get lists free here!</button>
  <div v-for="names in listnames" :key="names.id">
    <h5>
      {{ names.name }}
    </h5>
    <q-btn @click="loadTasks(names.id)">Get tasks</q-btn>
    <div class="q-pa-md" style="max-width: 100%">
    <q-list bordered class="rounded-borders">
      <div v-for="task in lists" :key="task.id">
        {{ task.name }}
      </div>
      </q-list>
      </div>
  </div>
</template>

<script>
export default {
  methods: {
    async getLists () {
      const listnames = await this.$axios.get('https://localhost:7096/List/getAll')
      console.log(listnames.data)
      this.listnames = listnames.data
    },
    async loadTasks (input) {
      const lists = await this.$axios.get('https://localhost:7096/List/getlist/' + input)
      console.log(lists.data)
      this.lists = lists.data
    }
  },
  data () {
    return {
      listnames: '',
      lists: '',
      newlist: { name: '' }
    }
  }
}
</script>
