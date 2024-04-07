<template>
  <div :class="formShow ? 'wrapper_blur' : ''">
    <h1>Мероприятия</h1>
    <div v-if="user" class="buttons">
      <MyButton v-if="!formShow" @click="openEmptyForm">Добавить событие</MyButton>
      <MyButton @click="logout">Выйти</MyButton>
    </div>
    <div v-else>
      <MyButton @click="auth">Авторизоваться</MyButton>
    </div>

    <div class="cards" v-if="data?.data">
      <EventItem v-for="(item, index) in data.data" :index="index" :user="user" :event="item" @update="refresh" @updateForm="sendCardToForm"></EventItem>
    </div>
    <div v-else>Нет данных, {{ error }}</div>
  </div>
  <EventForm v-if="formShow && user" @update="refreshList" :event="eventCard" @close="closeForm" @delete="deleteEvent" ></EventForm>
</template>

<script setup lang="ts">
    import { ref } from 'vue'
    import EventItem from "./EventItem.vue";
    import EventForm from "./EventForm.vue";
    import MyButton from "./UI/MyButton.vue";
    const router = useRouter();
    const client = useSupabaseClient()
    const events = ref([])
    const eventCard = ref({})
    const formShow = ref(false)
    const { data, pending, error, refresh } = await useAsyncData('events', () => client.from('events').select('*').order('id', { ascending: true }) )
    const user = useSupabaseUser()

    function toggleForm() {
      formShow.value = !formShow.value
    }
    function openForm() {
      formShow.value = true
    }
    function closeForm() {
      formShow.value = false
    }
    function openEmptyForm() {
      openForm()
      eventCard.value = null
    }
    function sendCardToForm(id: number) {
      openForm()
      eventCard.value = (data.value.data.filter((current: { id: number; }) => current.id === id))[0]

    }
    async function logout() {
      await client.auth.signOut()
    }
    function auth() {
      router.push("/login");
    }
    function refreshList() {
      closeForm()
      refresh();
    }
    async function deleteEvent(id: number) {
      await client.from('events').delete().eq('id', id)
      closeForm()
      refresh();
    }

</script>

<style>
  .cards {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 20px;
    margin-top: 10px;
  }
  .buttons {
    display: flex;
    gap: 20px;
    justify-content: space-between;
  }
  .wrapper_blur {
    filter: blur(10px);
  }
</style>
