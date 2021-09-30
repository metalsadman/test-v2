<template>
  <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    />
    <q-btn label="Test Dialog" @click="testDialog" />
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { ref } from 'vue';
import AddItemDialog from 'components/CustomDialog.vue';

const q$ = useQuasar();
const activeDialog = ref(null);

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
</script>
