<script>
import { workspaceList } from '../../store/global.js'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    newColumnName: '',
    workspaceName: '',
    board: {
      columns: []
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
            <li v-for="item in column.items" :key="item.id" class="base-card">
              {{ item.name }}
            </li>
          </ul>
        </section>
      </div>
    </section>
  </main>
</template>

<style>
.base-card {
  border: 1px solid #222;
  padding: 10px;
  list-style: none;
  background-color: #fff;
}

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
