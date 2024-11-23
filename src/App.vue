<template>
  <div class="flex justify-center items-center w-full h-screen">
    <div class="w-fit h-fit flex justify-center items-center">
      <Card class="w-full h-full">
        <template #title
          >Money Calculate <span class="pi pi-wallet"></span>
        </template>
        <template #content>
          <Form class="flex justify-center flex-col gap-4">
            <div class="flex justify-end w-full">
              <FloatLabel variant="on">
                <InputNumber
                  v-model="discount"
                  inputId="discount"
                  :minFractionDigits="2"
                  :maxFractionDigits="5"
                  fluid
                />
                <label for="on_label">Discount</label>
              </FloatLabel>
            </div>
            <DataTable :value="persons">
              <Column field="name" header="Name" class="w-[14rem]">
                <template #body="props">
                  <InputText
                    v-model="props.data.name"
                    name="name"
                    type="text"
                  />
                </template>
              </Column>
              <Column field="Amount" header="Amount" class="w-[14rem]">
                <template #body="props">
                  <InputNumber
                    v-model="props.data.amount"
                    inputId="Amount"
                    :minFractionDigits="2"
                    :maxFractionDigits="5"
                    fluid
                  />
                </template>
              </Column>
              <!-- <Column
                field="Percentage"
                header="Percentage (%)"
                class="w-[14rem]"
              >
                <template #body="props">
                  <InputNumber
                    v-model="props.data.percentage"
                    disabled
                    inputId="Percentage"
                    suffix=" %"
                    fluid
                  />
                </template>
              </Column> -->
              <!-- <Column
                field="DiscountAmount"
                header="Discount Amount"
                class="w-[14rem]"
              >
                <template #body="props">
                  <InputNumber
                    v-model="props.data.disAmount"
                    disabled
                    inputId="disAmount"
                    fluid
                  />
                </template>
              </Column> -->
              <!-- <Column
                field="Total Amount"
                header="Total Amount"
                class="w-[14rem]"
              >
                <template #body="props">
                  <InputNumber
                    v-model="props.data.totalAmount"
                    disabled
                    inputId="totalAmount"
                    fluid
                  />
                </template>
              </Column> -->
              <Column field="Delete" header="" class="w-max">
                <template #header="">
                  <Button
                    @click="addRow()"
                    icon="pi pi-plus"
                    aria-label="plus"
                  />
                </template>
                <template #body="props">
                  <Button
                    icon="pi pi-trash"
                    aria-label="trash"
                    severity="danger"
                    @click="removeRow(props.index)"
                  />
                </template>
              </Column>
            </DataTable>
            <Button @click="onFormSubmit" label="Create Bill" />
          </Form>
          <div class="w-full flex justify-end mt-2 text-xs gap-1">
            <a
              class="flex gap-1"
              href="https://github.com/tanawatchaijaroenice"
              target="_blank"
            >
              <Image
                src="https://avatars.githubusercontent.com/u/153838512?v=4"
                alt="Image"
                width="20"
              />
              tanawatchaijaroenice
            </a>
          </div>
        </template>
      </Card>
      <Bill :discount="discount" :persons="persons" v-model:visible="visible" />
    </div>
  </div>
</template>

<script setup lang="ts">
import InputNumber from "primevue/inputnumber";
import InputText from "primevue/inputtext";
import Card from "primevue/card";
import { ref } from "vue";
import FloatLabel from "primevue/floatlabel";
import Button from "primevue/button";
import DataTable from "primevue/datatable";
import Column from "primevue/column";
import Image from "primevue/image";
import Bill from "./components/Bill.vue";

const discount = ref(null);
const visible = ref(false);

const persons = ref<Person[]>([
  {
    id: 1,
    name: "",
    amount: null,
    percentage: 0,
    disAmount: 0,
    totalAmount: 0,
  },
  {
    id: 2,
    name: "",
    amount: null,
    percentage: 0,
    disAmount: 0,
    totalAmount: 0,
  },
]);

const addRow = () => {
  const newId = persons.value.length + 1;
  persons.value.push({
    id: newId,
    name: "",
    amount: null,
    percentage: 0,
    disAmount: 0,
    totalAmount: 0,
  });
};

const removeRow = (index: number) => {
  persons.value.splice(index, 1);
  calDistribution();
};

// (amount / sumAmount) * discount
const calDistribution = () => {
  const sumAmount = persons.value.reduce((a, b) => a + Number(b.amount), 0);
  const discountVal = discount.value!;

  persons.value.forEach((item) => {
    const rowAmount = item.amount!;
    const amountDivide = rowAmount / sumAmount;
    item.percentage = floatDigit(amountDivide * 100);
    item.disAmount = floatDigit(amountDivide * discountVal);
    item.totalAmount = floatDigit(rowAmount - item.disAmount);
  });
};

const floatDigit = (amount: number, digit = 2) => {
  return parseFloat(Number(amount).toFixed(digit));
};

const onFormSubmit = () => {
  calDistribution();
  visible.value = true;
};

console.log(persons.value);

interface Person {
  id: number;
  name: string;
  amount: number | null;
  percentage: number;
  disAmount: number;
  totalAmount: number;
}
</script>

<style>
.p-image {
  img {
    border-radius: 50%;
  }
}
</style>
