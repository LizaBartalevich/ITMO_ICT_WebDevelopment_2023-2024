<script setup>
import {onMounted, ref} from "vue";
import instance from "@/AxiosInstance";
import router from "@/router/router";
import {TokenStore} from "@/stores/TokenStore";

const Token = TokenStore()

const form = ref({
  name : "",
})


function getPaper(){
  instance.get(`/system/foods/${router.currentRoute.value.params.id}/`, {
    headers: {
      'Authorization': `Bearer ${Token.token}`
    }
  }).then(response => {
        if (response.status === 200){
          form.value = response.data
        }
      }
  ).catch(error => console.log(error))

}

function savePaper(){
  const {id, ...rest} = form.value
  instance.patch(`/system/foods/${router.currentRoute.value.params.id}/`, rest, {
    headers: {
      'Authorization': `Bearer ${Token.token}`
    }
  }).then(response => {
        if (response.status === 200){
          router.push('/foods')
        }
      }
  ).catch(error => console.log(error))
}


onMounted(() => {
  getPaper()
})


</script>

<template>
  <v-app>
    <div class="w-50 mx-auto">
      <h2>Тип диеты</h2>
      <v-text-field label="Название" v-model="form.name"></v-text-field>
      <v-btn @click="savePaper">Сохранить</v-btn>
    </div>
  </v-app>
</template>

<style scoped>

</style>

