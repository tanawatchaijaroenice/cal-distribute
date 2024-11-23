<template>
  <div class="card flex justify-center">
    <!-- <Button label="Show" @click="visible = true" /> -->
    <Dialog
      :visible="visible"
      @update:visible="updateVisible"
      modal
      header="Bill"
      :style="{ width: 'max-content' }"
      :breakpoints="{ '1199px': '75vw', '575px': '90vw' }"
    >
      <div class="flex justify-end">
        <div class="flex-auto w-full">
          <p for="Discount" class="font-bold text-xl text-right text-red-500">
            Discount: {{ discount }}
          </p>
        </div>
      </div>
      <div class="flex justify-end w-full">
        <DataTable :value="persons" tableStyle="min-width: 50rem">
          <Column field="name" header="Name" class="w-max">
            <template #body="props">
              <p>{{ props.data.name }}</p>
            </template>
          </Column>
          <Column field="Amount" header="Amount" class="w-[10rem]">
            <template #body="props">
              <p>{{ props.data.amount }}</p>
            </template>
          </Column>
          <Column field="Percentage" header="Percentage (%)" class="w-[10rem]">
            <template #body="props">
              <ProgressBar :value="props.data.percentage">
                {{ "" }}
              </ProgressBar>
              <!-- <p>{{ props.data.percentage }}</p> -->
            </template>
          </Column>
          <Column
            field="DiscountAmount"
            header="Disc. Amount"
            class="w-[10rem]"
          >
            <template #body="props">
              <p class="text-red-600">{{ props.data.disAmount }}</p>
            </template>
          </Column>
          <Column field="Total Amount" header="Total Amount" class="w-[10rem]">
            <template #body="props">
              <p class="text-green-600 font-semibold">
                {{ props.data.totalAmount }}
              </p>
            </template>
          </Column>
        </DataTable>
      </div>
    </Dialog>
  </div>
</template>

<script setup>
import InputNumber from "primevue/inputnumber";
import FloatLabel from "primevue/floatlabel";
import Button from "primevue/button";
import DataTable from "primevue/datatable";
import Column from "primevue/column";
import Dialog from "primevue/dialog";
import { ref, watch, defineProps, defineEmits } from "vue";
import ProgressBar from "primevue/progressbar";

// const props = defineProps({
//   persons: ref([]),
//   visible: ref(true),
// });

defineProps({
  visible: {
    type: Boolean,
    required: true,
  },
  persons: [],
  discount: "",
});

const emit = defineEmits(["update:visible"]);

const updateVisible = (value) => {
  emit("update:visible", value);
};

// watch(
//   () => visible,
//   (newVal) => {
//     if (!newVal) {
//       // Logic เพิ่มเติมเมื่อ Dialog ปิด (ถ้าจำเป็น)
//     }
//   }
// );
</script>
