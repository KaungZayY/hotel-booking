<template>
  <div>
    <h1 class="text-3xl font-light">Edit A Reservation</h1>
    <div class="[&>div]:mt-2">
      <div class="mb-3">
        <h2 class="mb-2 text-2xl">Room</h2>
        <div class="gap-x-3 flex mb-2">
          <div class="w-52 flex flex-col">
            <label for="room">Room Number</label>
            <MultiSelect v-model="reservationForm.room_id" :options="[...props.available_room, ...props.room_id]" filter
              placeholder="Select rooms" :maxSelectedLabels="5" class="md:w-20rem w-full" />
          </div>
          <div class="flex flex-col">
            <label for="totalPerson">Total Number Of Person</label>
            <InputNumber id="totoalPerson" v-model="reservationForm.total_person" />
          </div>
          <div class="flex flex-col">
            <label for="totalPrice">Total Price</label>
            <InputNumber id="totalPrice" v-model="reservationForm.total_price" mode="currency" currency="MMK" />
          </div>
        </div>
        <div class="gap-x-3 flex mb-2">
          <div class="flex flex-col">
            <label for="status">Status</label>
            <Textarea id="status" v-model="reservationForm.status" rows="5" cols="75" />
          </div>
        </div>
        <h3 class=" text-xl">Room Reservation Date</h3>
        <div class="gap-x-3 flex mt-2">
          <div class="flex flex-col">
            <label for="reservedFrom">From</label>
            <Calendar v-model="reservationForm.from_date" :minDate="minDate" :manualInput="false" id="reservedFrom" />
          </div>
          <div class="flex flex-col">
            <label for="reservedTo">To</label>
            <Calendar v-model="reservationForm.to_date" :minDate="minDate" :manualInput="false" id="reservedTo" />
          </div>
        </div>
      </div>
      <div class="mb-3">
        <div class="gap-x-3 flex">
          <div class="flex flex-col">
            <label for="checkin">Checkin</label>
            <Calendar v-model="reservationForm.checkin_time" :minDate="minDate" :manualInput="false" id="checkin"
              :showTime="true" hourFormat="24" />
          </div>
          <div class=" flex flex-col">
            <label for="checkout">Checkout</label>
            <Calendar v-model="reservationForm.checkout_time" :minDate="minDate" :manualInput="true" id="checkout"
              :showTime="true" hourFormat="24" />
          </div>
        </div>
      </div>
      <div class="mb-3">
        <Button label="Update Reservation" outlined @click="submitForm" />
      </div>
    </div>
  </div>
</template>

<script setup>
  import { useForm } from '@inertiajs/vue3';
  import Button from 'primevue/button';
  import Calendar from 'primevue/calendar';
  import InputNumber from 'primevue/inputnumber';
  import MultiSelect from 'primevue/multiselect';
  import Textarea from 'primevue/textarea';
  import { useToast } from "primevue/usetoast";

  import { ref } from 'vue';

  const props = defineProps({
    available_room: Array,
    room_id: Array,
    total_person: Number,
    total_price: Number,
    status: String,
    from_date: String,
    to_date: String,
    checkin_time: String,
    checkout_time: String,
  })
  const { available_room, ...editProps } = props

  const reservationForm = useForm({
    ...editProps
  })

  const toast = useToast();
  function submitForm() {
    reservationForm.put("/reservation", {
      onSuccess: () => toast.add({ severity: 'success', summary: 'Success', detail: 'Added Reservation Successfully', life: 3000 }),
    })
  }

  //For calculating min date
  const minDate = ref(new Date());

</script>