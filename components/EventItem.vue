<template>
<div class="card">
  <div class="card__top">
    <div class="card__title">{{ props.event.name }}</div>
    <div class="card__description">
      {{ props.event.description || 'Нет информации' }}
    </div>
  </div>
  <div class="card__bottom">
    <span class="card__span">Уже записалось: {{ props.event.numberParticipants || 0 }}</span>
    <MyButton class="card__button" @click="subscribe()">Записаться</MyButton>
    <MyButton v-if="user" class="card__button" @click="updateEvent()">Редактировать</MyButton>
  </div>
</div>
</template>

<script setup lang="ts">
  import MyButton from "./UI/MyButton"
  const emit = defineEmits<{
        (e: 'update', value: string): void
        (e: 'updateForm', value: number): void
    }>()
  const client = useSupabaseClient()
  type EventType = { id: number, name: string, description: string, date: Date, numberPaticipants: number }
  type UserType = { }
  type PropsType = { event: EventType, user: UserType }
  const props = defineProps<PropsType>()


  async function subscribe() {

      try {
        await client.from('events').update({numberParticipants: (props.event.numberParticipants + 1)}).eq('id', props.event.id).select();
        emit('update');
      } catch (e) {
        console.log(e)
      }
  }
  function updateEvent() {
    emit('updateForm', props.event.id)
  }
</script>

<style scoped lang="scss">
.card {
  padding: 15px;
  border-radius: 15px;
  color: #000000;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background: linear-gradient(180deg, rgba(203,163,255,1) 0%, rgba(197,255,176,1) 100%);

  &__title {
    font-weight: bold;
  }
  &__description {
    margin-top: 5px;
  }
  &__bottom {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 5px;
    gap: 5px;
  }
  &__button {
    cursor: pointer;
    border-radius: 5px;
    background: linear-gradient(180deg, rgba(203,163,255,1) 0%, rgba(197,255,176,1) 100%);

  }
}


</style>
