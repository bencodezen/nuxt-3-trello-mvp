<script>
import { workspaceList } from '../../store/global.js'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    workspaceName: ''
  }),
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
  </div>
</template>

<style></style>
