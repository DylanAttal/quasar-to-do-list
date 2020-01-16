<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newItem"
        @keyup.enter="addItem"
        class="col"
        bg-color="white"
        placeholder="Add Item"
        square
        filled
        dense
      >
        <template v-slot:append>
          <q-btn @click="addItem" icon="add" round dense flat />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(item, index) in items"
        :key="index"
        clickable
        @click="item.done = !item.done"
        :class="{ 'done bg-blue-1': item.done }"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="item.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ item.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="item.done" side>
          <q-btn
            @click.stop="deleteItem(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "First item",
          done: false
        },
        {
          title: "Second item",
          done: false
        },
        {
          title: "Third item",
          done: false
        }
      ],
      newItem: ""
    };
  },
  methods: {
    deleteItem(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Are you sure you want to delete this item?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.items.splice(index, 1);
          this.$q.notify("Item deleted.");
        });
    },
    addItem() {
      this.items.push({
        title: this.newItem,
        done: false
      });

      this.newItem = "";
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
