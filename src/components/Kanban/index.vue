<template>
  <div class="kanban">
    <h1 class="font-semibold text-xl px-4 mt-6">Список задач</h1>

    <div class="kanban_container">
      <div class="kanban_wrapper" ref="kanbanWrapper">
        <div class="kanban-main">
          <span class="scroll_dummy"></span>
          <div
            class="item-list mr-2"
            v-for="(el, index) in allData"
            :key="el.name"
          >
            <div
              class="py-2 mx-2 pl-3 flex justify-between items-center item-title"
            >
              <div class="font-semibold flex items-center title-badge">
                <div class="flex items-center">
                  <a-badge :color="el.color" />
                  <span> {{ el.name }}</span>
                </div>
                <span class="text-gray ml-2 count">
                  <template v-if="el.loading && el.page === 1">
                    <a-spin size="small" />
                  </template>
                  <template v-else>
                    {{ el.list.length }}
                  </template>
                </span>
              </div>
              <a-button @click="addTask(index)" type="dashed">+</a-button>
            </div>

            <div size="small" class="wrapper-item">
              <div class="scroll_wrap">
                <draggable
                  v-bind="dragOptions"
                  class="list-group"
                  :id="el.name"
                  :list="el.list"
                  group="tasks"
                  @end="end"
                  draggable=".task"
                  @change="change"
                >
                  <Item
                    class="mb-2 task"
                    v-for="element in el.list"
                    :key="element.id"
                    :item="element"
                  />
                </draggable>
              </div>
            </div>
          </div>
          <span class="scroll_dummy"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import Item from "./Item";
export default {
  components: { draggable, Item },
  data() {
    return {
      dragOptions: {
        animation: 200,
        ghostClass: "ghost",
      },
      selectElement: null,
      allData: [
        {
          name: "Новая",
          loading: false,
          color: "blue",
          list: [],
        },
        {
          name: "В работе",
          color: "purple",

          loading: false,
          list: [],
        },
        {
          name: "Завершенная",
          color: "orange",

          loading: false,
          list: [],
        },
      ],
    };
  },
  created() {
    let cacheData = JSON.parse(localStorage.getItem("taskList"));
    if (cacheData) this.allData = cacheData;
  },
  methods: {
    addTask(index) {
      const newEl = {
        name: `Задача ${Math.floor(Math.random() * 9999999)}`,
      };
      this.allData[index].list.push(newEl);
      this.saveTask();
    },
    saveTask() {
      localStorage.setItem("taskList", JSON.stringify(this.allData));
    },
    async end() {
      try {
        setTimeout(async () => {
          if (this.selectElement) {
            this.saveTask();
          }
        }, 200);
      } catch (error) {
        this.$message.error("Ошибка");
      }
    },
    async change(el) {
      try {
        let element = null;

        if (el.added) {
          element = el.added.element;
          this.selectElement = element;
        }

        if (el.moved) {
          element = el.moved.element;
          this.selectElement = element;
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped lang="scss">
.kanban {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  height: 100%;
  .kanban_button {
    padding-left: 30px;
    padding-right: 30px;
    padding-top: 20px;
  }
  .flip-list-move {
    transition: transform 0.5s;
  }
  .no-move {
    transition: transform 0s;
  }
  .ghost {
    opacity: 0.5;
    background: #c8ebfb;
    &:not(:last-child) {
      margin-bottom: 15px;
    }
  }
}
.scroll_wrap {
  padding-left: 7px;
  padding-right: 7px;
  height: 100%;
  margin-bottom: 50px;
  overflow-y: auto;
  overflow-x: hidden;
}
.kanban_wrapper {
  position: relative;
  padding-bottom: 20px;
  overflow-x: auto;
  overflow-y: hidden;
  display: flex;
  -webkit-overflow-scrolling: touch;
  overscroll-behavior-x: contain;
  flex-direction: column;
  --safe-area-inset-bottom: env(safe-area-inset-bottom);
  height: 100%;
  &::-webkit-scrollbar {
    height: 7px;
  }
  @media (min-width: 981px) {
    scroll-padding: 40px;
  }
  @media (max-width: 980px) {
    scroll-padding: 20px;
    scroll-snap-type: x mandatory;
  }
}
.kanban_container {
  flex-grow: 1;
  height: 90vh;
}
.kanban-main {
  display: flex;
  flex-grow: 1;
  flex-direction: row;
  height: 100%;

  .scroll_dummy {
    @media (min-width: 981px) {
      min-width: 30px;
    }
    @media (max-width: 980px) {
      min-width: 15px;
    }
  }
  .list-group {
    height: 100%;
    .scroll_wrap {
      padding-left: 7px;
      padding-right: 7px;
      padding-top: 7px;
      padding-bottom: 10px;
    }
  }

  .wrapper-item {
    height: calc(100% - 40px);
  }

  .item-list {
    min-width: 300px;
    max-width: 300px;
    height: 100%;
    scroll-snap-align: start;
    flex-grow: 0;
    background-color: #eff2f5;
    border-radius: var(--borderRadius);
    flex-shrink: 0;
    padding-bottom: 5px;
    overflow: hidden;
    .block_btn {
      margin-right: -7px;
    }
  }
}
</style>

<style lang="scss">
.scroll_block {
  .__view {
    height: 100%;
  }
}
.item-list {
  .ant-card {
    box-shadow: 0 1px 0 rgba(9, 30, 66, 0.15);
    border: 0px;
  }
}
</style>
