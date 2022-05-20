<template>
  <div class="home">
    <Kanban />
  </div>
</template>

<script>
// @ is an alias to /src
import Kanban from "@/components/Kanban";
export default {
  name: "HomeView",
  components: {
    Kanban,
  },
};
</script>
