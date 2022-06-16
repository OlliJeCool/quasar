
<template>
  <br>
  <p>Create new task</p>
  <div class="q-pa-md" style="max-width: 400px">

    <q-form
      @submit="createTask()"
      @reset="onReset()"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="newtask.name"
        label="Name *"
        hint="Enter a name."
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type something']"
      />

      <q-input
        filled
        type="description"
        v-model="newtask.description"
        label="Description *"
        lazy-rules
      />

      </q-form>
      <div>
        <br>
        <q-btn @click="createTask()" label="Submit" type="submit" color="primary"/>
      </div>
      </div>
    <div class="q-pa-md" style="max-width: 100%">
      <button @click="mounted()">Get tasks</button>
      <br>
    <q-list bordered class="rounded-borders">
      <div v-for="task in tasks" :key="task.id">
      <q-expansion-item
        expand-separator
        icon=done
        :label="task.name"
        header-class="bg-grey-2 text-black"
      >
        <q-card>
          <q-card-section>
            {{ task.description }}
          </q-card-section>
          <q-card-section>
            <q-fab padding="8px" color="secondary" push icon="keyboard_arrow_right" direction="right">
              <q-fab-action color="primary" @click="completeTask(task.id)" icon="done" />
              <q-fab-action color="red" @click="deleteTask(task.id)" icon="delete" />
            </q-fab>
            <div class="q-pa-md">
              <q-btn-dropdown width="50px" color="primary" label="Add to list">
                <q-list>
                  <div v-for="list in lists" :key="list.id">
                    <q-item clickable v-close-popup @click="addToList(task.id, list.id)">
                      <q-item-section>
                        <q-item-label>{{ list.name }}</q-item-label>
                      </q-item-section>
                    </q-item>
                  </div>
                </q-list>
              </q-btn-dropdown>
            </div>
          </q-card-section>
        </q-card>
      </q-expansion-item>
      </div>
      </q-list>
      </div>
</template>

<script>

export default {
  methods: {
    async getTasks () {
      const tasks = await this.$axios.get('https://localhost:7096/Task/GetTasks')
      console.log(tasks.data)
      this.tasks = tasks.data
    },
    async deleteTask (input) {
      await this.$axios.post('https://localhost:7096/Task/delete', {
        id: input
      })
    },
    async completeTask (input) {
      await this.$axios.post('https://localhost:7096/Task/complete', {
        id: input
      })
    },
    async createTask () {
      await this.$axios.post('https://localhost:7096/Task/create', {
        name: this.newtask.name,
        description: this.newtask.description
      })
    },
    async getLists () {
      const listnames = await this.$axios.get('https://localhost:7096/List/getAll')
      console.log(listnames.data)
      this.lists = listnames.data
    },
    async addToList (taskid, listid) {
      await this.$axios.post('https://localhost:7096/List/add', {
        ids: taskid,
        listId: listid
      })
    },
    mounted () {
      this.getTasks()
      this.getLists()
    }
  },
  data () {
    return {
      tasks: '',
      newtask: { name: '', description: '' },
      lists: ''
    }
  }
}
</script>
