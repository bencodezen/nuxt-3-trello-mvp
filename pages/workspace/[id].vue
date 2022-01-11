<script>
import { workspaceList } from '../../store/global.js'
import { UseDraggable } from '@vueuse/components'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  components: {
    UseDraggable
  },
  data: () => ({
    newColumnName: '',
    workspaceName: '',
    board: {
      columns: [
        {
          columnName: 'Luffy',
          newItemName: '',
          items: [
            {
              id: 123,
              name: 'Rocket Punch'
            }
          ]
        },
        {
          columnName: 'Zorro',
          newItemName: '',
          items: []
        },
        {
          columnName: 'Nani',
          newItemName: '',
          items: []
        }
      ]
    }
  }),
  methods: {
    createColumn() {
      if (this.newColumnName) {
        this.board.columns.push({
          columnName: this.newColumnName,
          newItemName: '',
          items: []
        })

        this.newColumnName = ''
      }
    },
    createCard(column) {
      if (column.newItemName) {
        column.items.push({
          id: 123,
          name: column.newItemName
        })

        column.newItemName = ''
      }
    },
    migrateItem({ cardId, targetColumn, originalColumn }) {
      // Step 1. Move item to new list
      this.board.columns
        .find(column => column.columnName === targetColumn)
        .items.push(
          this.board.columns
            .find(column => column.columnName === originalColumn)
            .items.find(item => item.id === cardId)
        )

      // Step 2. Delete item from old list
      const parentColumn = this.board.columns.find(
        column => column.columnName === originalColumn
      )

      parentColumn.items = parentColumn.items.filter(item => item.id !== cardId)
    }
  },
  mounted() {
    this.workspaceName = this.workspaceList.find(
      workspace => workspace.id === Number(this.$route.params.id)
    ).name
  }
}
</script>

<template>
  <main class="workspace-page">
    <h1>{{ workspaceName }} Workspace (#{{ $route.params.id }})</h1>
    <section>
      <input
        v-model="newColumnName"
        type="text"
        placeholder="New column title"
        style="
          display: block;
          width: 100%;
          padding: 5px;
          font-size: 0.9rem;
          margin-bottom: 10px;
        "
        @keyup.enter="createColumn"
      />
      <button @click="createColumn" style="margin-bottom: 15px">
        Create Column
      </button>
      <div class="column-grid">
        <section class="board-column" v-for="column in board.columns">
          <h3 style="margin-top: 5px">{{ column.columnName }}</h3>
          <input
            v-model="column.newItemName"
            @keyup.enter="createCard(column)"
            type="text"
            style="padding: 5px; font-size: 0.9rem; margin-bottom: 10px"
            placeholder="New card title"
          />
          <button @click="createCard(column)" style="margin-bottom: 15px">
            Create Card
          </button>
          <ul style="margin: 0; padding: 0">
            <li
              v-for="item in column.items"
              :key="item.id"
              style="list-style: none"
            >
              <UseDraggable :initialValue="{ x: 41, y: 10 }" v-slot="{ x, y }">
                <BaseCard
                  :data="item"
                  :parentColumn="column.columnName"
                  :migrateList="board.columns"
                  @migrate-item="migrateItem"
                  :position="{ x: x, y: y }"
                />
              </UseDraggable>
            </li>
          </ul>
        </section>
      </div>
    </section>
  </main>
</template>

<style scoped>
.column-grid {
  display: grid;
  grid-template-columns: repeat(v-bind(board.columns.length), 1fr);
}

.board-column {
  background-color: #eee;
  border: 1px solid #222;
  height: 80vh;
  margin-right: 1rem;
  padding: 10px;
}

.workspace-page {
  padding: 30px;
}

.workspace-page h1 {
  margin-top: 0;
}
</style>
