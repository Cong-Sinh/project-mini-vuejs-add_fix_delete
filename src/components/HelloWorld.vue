<template>
  <div>
    <h1>Quản lí sinh viên</h1>
    <student :itemEdit="hocsinh" @save="clickAdd" />
    <b-table :items="items" :fields="fields" responsive="sm" ref="selectableTable" selectable
      @row-selected="onRowSelected">
      <!-- Example scoped slot for select state illustrative purposes -->
      <template #cell(selected)="{ rowSelected }">
        <template v-if="rowSelected">
          <span aria-hidden="true">&check;</span>
          <span class="sr-only">Selected</span>
        </template>
        <template v-else>
          <span aria-hidden="true">&nbsp;</span>
          <span class="sr-only">Not selected</span>
        </template>
      </template>
    </b-table>
    <p>
      <b-button size="sm" @click="selectAllRows">Select all</b-button>
      <b-button size="sm" @click="clearSelected">Clear selected</b-button>
      <b-button @click="clickEdit">Edit</b-button>
      <b-button @click="clickDelete">Delete</b-button>
    </p>
    <p>
      Selected Rows:<br>
      {{ selected }}
    </p>
  </div>
</template>

<script>
import Student from './Student.vue'

export default {
  components: {
    Student
  },
  data() {
    return {
      hocsinh: {
      },
      fields: ['selected', 'id', 'name', 'age', 'phone', "adress"],
      items: [
        {
          id: 1,
          name: "Tran Van Hung",
          age: 20,
          phone: 12323242452,
          adress: "HN"
        },
        {
          id: 2,
          name: "Nguyen Duc Manh",
          age: 20,
          phone: 777777,
          adress: "HCM"
        },
        {
          id: 3,
          name: "Quang Dang ",
          age: 24,
          phone: 44444444,
          adress: "Soc Trang"
        },
        {
          id: 4,
          name: "Duong Hoang Yen",
          age: 18,
          phone: 99999999,
          adress: "Hai Phong"
        },
      ],
      selectMode: 'multi',
      selected: []
    }
  },
  methods: {
    clickAdd(itemSave) {
      let index = this.items.findIndex((c) =>
        c.id === itemSave.id)
      if (index >= 0) {
        this.items.splice(index, 1, itemSave)
      } else {
        this.items.push(itemSave)
      }
      return
    },
    clickEdit() {
      if (this.selected.length === 1) {
        this.hocsinh = this.selected[0]
        this.$bvModal.show("modal-prevent-closing")
      }
    },
    delete(id) {
      for (let i = 0; i < this.items.length; i++) {
        if (id === this.items[i].id) {
          this.items.splice(i, 1)
        }
      }
    },
    clickDelete() {
      if (this.selected.length >= 1) {
        for (let i = 0; i < this.selected.length; i++) {
          this.delete(this.selected[i].id)
        }
      }
    },
    onRowSelected(items) {
      this.selected = items
    },
    selectAllRows() {
      this.$refs.selectableTable.selectAllRows()
    },
    clearSelected() {
      this.$refs.selectableTable.clearSelected()
    },
  }
}
</script>