<template>
  <div id="app">
    <form style="padding: 24px;">
      First Name : <input type="text" v-model="form.firstname"><br><br>
      Last Name : <input type="text" v-model="form.lastname"><br><br>
      Age : <input type="text" v-model="form.age"><br><br>
      <button
        type="button"
        class="btn btn-primary"
        @click="add()">
        ADD
      </button>
      <button
        type="button"
        class="btn btn-secondary"
        @click="update()">
        UPDATE
      </button>
    </form>
    <table class="table">
      <thead>
        <tr>
          <th> First Name </th>
          <th> Last Name </th>
          <th> Age </th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, i) in items" :key="i">
          <td>
            {{ item.firstname }}
          </td>
          <td>
            {{ item.lastname }}
          </td>
          <td>
            {{ item.age }}
          </td>
          <td>
            <button
              type="button"
              style="margin-right: 20px;"
              class="btn btn-dark"
              @click="set(item)">
              Set
            </button>
            <button
              type="button"
              class="btn btn-danger"
              @click="remove(item.id)">
              delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      form: {
        firstname: '',
        lastname: '',
        age: null
      },
      items: []
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    setDefault () {
      this.form = {
        id: null,
        firstname: '',
        lastname: '',
        age: null
      }
    },
    async getData () {
      const { data } = await axios.get('https://61094d2fd71b670017639843.mockapi.io/users')
      this.items = data
    },
    async add () {
      await axios.post('https://61094d2fd71b670017639843.mockapi.io/users', {
        firstname: this.form.firstname,
        lastname: this.form.lastname,
        age: Number(this.form.age)
      })
      this.getData()
      this.setDefault()
      alert('Create Success!!')
    },
    async remove (id) {
      await axios.delete(`https://61094d2fd71b670017639843.mockapi.io/users/${id}`)
      this.getData()
      alert('Delete Success!!!')
    },
    set (item) {
      this.form = {
        id: item.id,
        firstname: item.firstname,
        lastname: item.lastname,
        age: item.age
      }
    },
    async update () {
      await axios.put(`https://61094d2fd71b670017639843.mockapi.io/users/${this.form.id}`, {
        id: this.form.id,
        firstname: this.form.firstname,
        lastname: this.form.lastname,
        age: Number(this.form.age)
      })
      this.getData()
      this.setDefault()
      alert('Delete Success!!!')
    }
  }
}
</script>
