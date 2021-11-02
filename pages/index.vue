<script lang="ts" setup>
import { ref } from 'vue'

interface Workspace {
  id: Number
  name: String
}

const newWorkspaceName = ref('')
const workspaceList = ref<Workspace[]>([])
const createWorkspace = () => {
  const randomId = Math.floor(Math.random() * 100)

  workspaceList.value.push({
    id: randomId,
    name: newWorkspaceName.value
  })

  newWorkspaceName.value = ''
}
</script>

<template>
  <h1>Home Page</h1>
  <h2>Recently Viewed</h2>
  <h2>Workspaces</h2>
  <input
    type="text"
    v-model="newWorkspaceName"
    @keyup.enter="createWorkspace"
  />
  <button @click="createWorkspace">Create a Workspace</button>
  <ul class="workspace-list">
    <li
      v-for="workspace in workspaceList"
      :key="workspace.id"
      class="workspace-card"
    >
      {{ workspace.id }}: {{ workspace.name }}
    </li>
  </ul>
</template>

<style>
.workspace-card {
  display: block;
  border: 2px solid #222;
  border-radius: 4px;
  padding: 2rem;
  margin-bottom: 1rem;
}

.workspace-list {
  margin-left: 0;
  padding-left: 0;
}
</style>
