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

<script setup>
import { useDialogPluginComponent, useQuasar } from 'quasar';
import { ref, computed } from 'vue';

const props = defineProps({
  callback: {
    type: Function,
    required: true,
  },
});
const emit = defineEmits([
  // REQUIRED; need to specify some events that your
  // component will emit through useDialogPluginComponent()
  ...useDialogPluginComponent.emits,
]);

const todo = ref(''),
  input = ref(null),
  q$ = useQuasar();

// const dialogRef = useDialogPluginComponent().dialogRef;
// const onDialogHide = useDialogPluginComponent().onDialogHide;

const { dialogRef, onDialogHide, onDialogOK, onDialogCancel } =
  useDialogPluginComponent();

console.log('dialogRef', dialogRef.value);
console.log('dialogHide', onDialogHide);
console.log('useDialogPluginComponent', useDialogPluginComponent);

// defineExpose({
//   dialogRef,
//   onDialogHide,
//   onDialogOK,
//   onDialogCancel,
// });

function onCancel() {
  todo.value = '';
  props.callback('cancel', todo.value);
  // useDialogPluginComponent().onDialogCancel();
  onDialogCancel();
}

function onAddAnother() {
  input.value.focus();
  props.callback('addanother', todo.value);
  todo.value = '';
}

function onAdd() {
  props.callback('add', todo.value);
  // useDialogPluginComponent().onDialogOK();
  onDialogOK();
}

const color = computed(() => {
  return q$.dark.isActive ? 'warning' : 'primary';
});
</script>
