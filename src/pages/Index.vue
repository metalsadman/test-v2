<template>
  <q-page>
    <div class="column">
      <div class="col">
        <img
          alt="Quasar logo"
          src="~assets/quasar-logo-vertical.svg"
          style="width: 200px; height: 200px"
        />
      </div>
      <div class="col">
        <q-btn label="Test Dialog w/ script setup" @click="testDialog" />
        <q-btn label="Test Dialog w/o Script setup" @click="testDialog2" />
      </div>
      <div class="col">
        <q-btn label="Close Dialog" @click="closeDialog" />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { ref } from 'vue';
import AddItemDialog from 'components/CustomDialog.vue';
import AddItemDialog2 from 'components/CustomDialogCompo.vue';

const q$ = useQuasar();
const activeDialog = ref(null);

function testDialog() {
  activeDialog.value = createDialog(AddItemDialog);
  console.log('testDialog1', activeDialog.value);
}

function testDialog2() {
  activeDialog.value = createDialog(AddItemDialog2);

  console.log('testDialog2', activeDialog.value);
}

function createDialog(customComponent) {
  return q$.dialog({
    component: customComponent,
    componentProps: {
      callback: (btnClicked, input) => {
        if (btnClicked === 'cancel') {
          console.log('callBack called cancel');
          activeDialog.value = null;
          return;
        }
        //do something...
      },
    },
  });
}

function closeDialog() {
  console.log('closeDialog', activeDialog.value);
  activeDialog.value.hide();
}
</script>
