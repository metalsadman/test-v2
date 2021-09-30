<template>
  <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    />
    <q-btn label="Test Dialog" @click="testDialog" />
    <q-btn label="Close Dialog" @click="closeDialog" />
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { ref } from 'vue';
import AddItemDialog from 'components/CustomDialog.vue';

const q$ = useQuasar();
const activeDialog = ref(null);

function testDialog() {
  if (!activeDialog.value) return;
  activeDialog.value = q$.dialog({
    component: AddItemDialog,
    componentProps: {
      callback: (btnClicked, input) => {
        if (btnClicked === 'cancel') {
          activeDialog.value = null;
          return;
        }
        //do something...
      },
    },
  });
}

function closeDialog() {
  activeDialog.value.hide();
}
</script>
