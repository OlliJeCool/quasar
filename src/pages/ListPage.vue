
<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-btn label="Create list" color="primary" @click="create = true" />
      <q-dialog v-model="create">
        <q-card>
          <q-card-section>
            <div>
                <q-form
      @submit="createList()"
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
      <q-btn @click="deleteList(names.id)" color="red" icon="delete">Delete</q-btn>
    </div>
    <div class="q-pa-md" style="max-width: 100%">
    <q-list bordered class="rounded-borders">
      </q-list>
      </div>
  </div>
</template>

<script>
export default {
  mounted () {
    this.getLists()
    this.getTasks()
  },
  updated () {
    this.getLists()
    this.getTasks()
  },
  methods: {
    async getLists () {
      const listnames = await this.$axios.get('https://localhost:7096/List/getAll')
      console.log(listnames.data)
      this.listnames = listnames.data
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
    },
    async getTasks () {
      for (let i = 0; i < this.listnames.length; i++) {
        const tasks = await this.$axios.get('https://localhost:7096/List/getlist/' + this.listnames[i].id)
        for (let j = 0; i < tasks.data.length; j++) {
          this.taskslist[this.listnames[i].id] = {
            id: tasks.data[j].id,
            name: tasks.data[j].name,
            description: tasks.data[j].description
          }
        }
      }
      console.log(this.tasklist)
    }
  },
  data () {
    return {
      listnames: '',
      newlist: { name: '' },
      create: false,
      tasklist: {}
    }
  }
}
</script>
