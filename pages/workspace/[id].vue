<script>
import { workspaceList } from '../../store/global.js'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    workspaceName: '',
    board: {
      name: 'Cherries',
      columns: []
    }
  }),
  methods: {
    createColumn() {
      this.board.columns.push({
        items: []
      })
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
  <div>
    <h1>{{ workspaceName }} Workspace (#{{ $route.params.id }})</h1>
    <section>
      <h2>{{ board.name }}</h2>
      <button @click="createColumn">Create Column</button>
      <div class="column-grid">
        <ul class="board-column" v-for="column in board.columns"></ul>
      </div>
    </section>
  </div>
</template>

<style>
.column-grid {
  display: grid;
  grid-template-columns: repeat(v-bind(board.columns.length), 1fr);
}

.board-column {
  border: 1px solid #222;
  height: 80vh;
  width: 100px;
  margin-right: 1rem;
}
</style>
