<template>
  <div class="modal">
    <div class="event-form modal-content">
      <label>
        <input v-model="eventName" placeholder="Название события" />
      </label>
      <label>
        <input v-model="eventDescription" placeholder="Описание события" />
      </label>
      <label>
        <input v-model="eventDate" type="date" placeholder="Дата события" />
      </label>

      <MyButton v-if="isNotEmptyFields" @click="handleSubmit">
        <template v-if="props.event">Сохранить изменения</template>
        <template v-else>Создать событие</template>
      </MyButton>
      <MyButton @click="returnForm">Отменить изменения</MyButton>

      <Close class="modal__close" @click="closeForm">Закрыть форму</Close>
    </div>
  </div>
</template>

<script setup lang="ts">
    import Close from "../assets/img/close"
    import MyButton from "./UI/MyButton"
    import {computed} from 'vue'
    const emit = defineEmits<{
      (e: 'update', value: string): void
      (e: 'close', value: string): void
    }>()

    type PropsType = { event: EventType }
    const props = defineProps<PropsType>()
    const client = useSupabaseClient()

    const eventName = ref(props.event?.name) || ref('')
    const eventDescription = ref(props.event?.description) || ref('')
    const eventDate = ref(props.event?.date) || ref('')
    const oldDataValue = ref({
      name: eventName.value,
      description: eventDescription.value,
      date: eventDate.value,
    })
    const isNotEmptyFields = computed(() => eventName.value && eventDescription.value && eventDate.value)
    function closeForm() {
      emit('close')
    }
    function returnForm() {
      eventName.value = oldDataValue.value.name
      eventDescription.value = oldDataValue.value.description
      eventDate.value = oldDataValue.value.date
    }
    async function handleSubmit() {

      if (!props.event) {
        await client.from('events').insert({name: eventName.value, description: eventDescription.value, date: eventDate.value});
      } else {
        await client.from('events').update({name: eventName.value, description: eventDescription.value, date: eventDate.value}).eq('id', props.event.id);
      }
      emit('update');
    }
</script>
<style lang="scss">
  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    border-radius: 15px;
    &__close {
      position: absolute;
      top: 10px;
      right: 20px;
      cursor: pointer;
    }
  }

  .modal-content {
    text-align: center;

  }
  .event-form {
    display: flex;
    flex-direction: column;
    margin-top: 12px;
    width: fit-content;
    margin-left: auto;
    margin-right: auto;
    gap: 5px;
    input {
      width: 100%;
      width: -moz-available;
      width: -webkit-fill-available;
      width: fill-available;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #C8C8C8;
      border-radius: 8px;
      padding: 8px;
      height: 10px;
    }
    input:focus {
      background-color: #ffffff;
      outline-width: 0;
    }
  }
</style>

