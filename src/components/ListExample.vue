<template>
  <div>
    <div
        class="feed-back-name"
    >
      <input
          placeholder="Имя"
          v-model="formData.name"
      >
      <div
          class="feed-back-name-error"
      >
        {{ errors.name }}
      </div>
    </div>
    <div
        class="feed-back-phone"
    >
      <input
        placeholder="номер телефона"
        v-model="formData.phone"
      >
      <div
          class="feed-back-phone-error"
      >
        {{ errors.phone }}
      </div>
    </div>
    <div
        class="feed-back-form"
    >
      <textarea
          placeholder="Отзыв"
          v-model="formData.feedback"
      ></textarea>
      <div
          class="feed-back-form-error"
      >
        {{ errors.feedback }}
      </div>
    </div>
    <div>
    <input
        name="save-path"
        type="radio"
        value="file"
        v-model="formData.saveTo"
        checked
    >
    Файловое хранилище
    </div>
    <div>
    <input
        name="save-path"
        type="radio"
        value="database"
        v-model="formData.saveTo"
    >
    База данных
    </div>
    <div
        v-on:click="submitForm()"
    >
      Отправить
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "ListExample",
  data () {
    return {
      errors: {
        name: '',
        phone: '',
        feedback: '',
      },
      formData: {
        name: '',
        phone: '',
        feedback: '',
        saveTo: 'file',
      },
    }
  },
  methods: {
    submitForm() {
      this.errors = {};
      axios.post(`http://127.0.0.1:8000/api/feedback/save`, this.formData)
          .then(() => {
            alert('Ваш запрос успешно отправлен на сервер');
            window.location.reload();
          })
          .catch((err) => {
            this.handleError(err.response.data);
          })
    },
    handleError(err) {
      if (err.errors?.phone?.length > 0) {
        this.errors.phone = err.errors.phone[0]
      }
      if (err.errors?.name?.length > 0) {
        this.errors.name = err.errors.name[0]
      }
      if (err.errors?.feedback?.length > 0) {
        this.errors.feedback = err.errors.feedback[0]
      }
    },
  }
}
</script>

<style scoped>

</style>