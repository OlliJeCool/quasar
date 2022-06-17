
<template>
  <div v-for="names in listnames" :key="names.id">
    <h5>
      {{ names.name }}
    </h5>
    <div class="q-pa-md" style="max-width: 100%">
    <q-list bordered class="rounded-borders">
      <div v-for="task in loadTasks(names.id)" :key="task.id">
        {{ task.name }}
      </div>
      </q-list>
      </div>
  </div>
</template>

<script>
export default {
  mounted () {
    this.getLists()
  },
  methods: {
    async getLists () {
      const listnames = await this.$axios.get('https://localhost:7096/List/getAll')
      console.log(listnames.data)
      this.listnames = listnames.data
    },
    async loadTasks (input) {
      const lists = await this.$axios.get('https://localhost:7096/List/getlist/' + input)
      console.log(lists.data)
      return lists.data
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
