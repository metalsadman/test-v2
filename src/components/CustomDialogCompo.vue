<template>
  <q-dialog ref="dialogRef" @hide="onDialogHide" seamless persistent>
    <q-card class="q-dialog-plugin">
      <q-card-section>
        <div class="text-h6">Add item</div>
      </q-card-section>
      <q-card-section class="q-py-none">
        <div>Type item name</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <q-input
          ref="input"
          :color="color"
          dense
          v-model="todo"
          autofocus
        ></q-input>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Cancel" :color="color" @click="onCancel"></q-btn>
        <q-btn
          flat
          label="Add another"
          :color="color"
          @click="onAddAnother"
        ></q-btn>
        <q-btn flat label="Add" :color="color" @click="onAdd"></q-btn>
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { useDialogPluginComponent, useQuasar } from 'quasar';
import { ref, computed, defineComponent } from 'vue';

export default defineComponent({
  props: {
    callback: {
      type: Function,
      required: true,
    },
  },
  emits: [
    // REQUIRED; need to specify some events that your
    // component will emit through useDialogPluginComponent()
    ...useDialogPluginComponent.emits,
  ],
  setup(props) {
    const todo = ref(''),
      input = ref(null),
      q$ = useQuasar();

    const { dialogRef, onDialogHide, onDialogOK, onDialogCancel } =
      useDialogPluginComponent();

    console.log('dialogRef', dialogRef.value);

    function onCancel() {
      todo.value = '';
      props.callback('cancel', todo.value);
      onDialogCancel();
    }

    function onAddAnother() {
      input.value.focus();
      props.callback('addanother', todo.value);
      todo.value = '';
    }

    function onAdd() {
      props.callback('add', todo.value);
      onDialogOK();
    }

    const color = computed(() => {
      return q$.dark.isActive ? 'warning' : 'primary';
    });
    return {
      onAddAnother,
      onCancel,
      color,
      onAdd,
      dialogRef,
      onDialogHide,
      todo,
      input,
    };
  },
});
</script>
