<template>
  <div class="about">
    <button @click="addItem">Add Item</button>
    <div class="list-wrapper">
      <virtual-drag-list
        :data-source="listData"
        :data-key="'id'"
        :keeps="20"
        :size="50"
        :scroller="scroller"
        @drag="dragStart"
        @drop="dragEnd"
      >
        <template slot="item" slot-scope="{ record, index }">
          <div class="list-item">
            <div>{{ index }} - {{ record.title }}</div>
            <div>{{ record.content }}</div>
          </div>
        </template>
      </virtual-drag-list>
    </div>
  </div>
</template>

<script>
import {
  defineComponent, ref, onMounted, nextTick,
} from 'vue';
import VirtualDragList from 'vue-virtual-draglist';

export default defineComponent({
  name: 'about-page',
  components: {
    VirtualDragList,
  },
  setup() {
    const listData = ref([]);
    const scroller = ref(null);

    const dragStart = () => {
      console.log('drag start');
    };

    const dragEnd = ({ list }) => {
      listData.value = [...list];
    };

    const addItem = () => {
      const item = {
        id: listData.value.length,
        title: `list-${listData.value.length}`,
        content: 'some content.',
      };
      listData.value = [
        ...listData.value,
        item,
      ];
    };

    onMounted(() => {
      nextTick(() => {
        [scroller.value] = document.getElementsByClassName('list-wrapper');
        console.log('scroller: ', scroller.value);
      });
    });

    return {
      listData,
      scroller,
      dragStart,
      dragEnd,
      addItem,
    };
  },
});

</script>

<style lang="scss" scoped>
  .about {
    height: 100%;
    position: relative;
    padding: 10px;
  }
  .list-wrapper {
    margin-top: 10px;
    height: calc(100vh - 100px);
    border: 1px slateblue solid;
    overflow-y: scroll;
  }
  .list-item {
    height: 50px;
    border: 1px gray solid;
    margin: 10px;
  }
</style>
