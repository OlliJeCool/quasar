
<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-btn label="Create task" color="primary" @click="create = true" />
      <q-dialog v-model="create">
        <q-card>
          <q-card-section>
            <div>
                <q-form
      @submit="createTask()"
      @reset="onReset()"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="newlist.name"
        label="Name *"
        hint="Enter a name."
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type something']"
      />

      </q-form>
      <div>
        <br>
        <q-btn @click="createList()" label="Submit" type="submit" color="primary"/>
      </div>
      </div>
          </q-card-section>
        </q-card>
      </q-dialog>
  </div>
  <div v-for="names in listnames" :key="names.id">
    <h5>
      {{ names.name }}
    </h5>
    <div v-if="names.name !== 'MainList' && names.name !== 'CompletedList'">
      <q-btn @click="deleteTask(names.id)" color="red" icon="delete">Delete</q-btn>
    </div>
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
  updated () {
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
    },
    async createList () {
      await this.$axios.post('https://localhost:7096/List/create', {
        name: this.newlist.name
      })
      this.created = false
    },
    async deleteList (input) {
      console.log(input)
      await this.$axios.post('https://localhost:7096/List/delete', {
        id: input
      })
      this.getLists()
    }
  },
  data () {
    return {
      listnames: '',
      lists: '',
      newlist: { name: '' },
      created: false
    }
  }
}
</script>
