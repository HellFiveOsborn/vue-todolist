<script>
import TodoItems from "@components/Todo/Items.vue";

export default {
  name: "TodoList",
  components: {
    TodoItems,
  },
  data() {
    return {
      showAlert: [],
      newContent: "",
      readonly: true,
      contents: [
        {
          title: 'Estudar VueJS',
          body: '<i>Vue (pronunciado /vjuː/, como view ) é uma abstração de JavaScript para construção de interfaces de utilizador. Ela constrói sobre a HTML, CSS e JavaScript padrão, e fornece um modelo de programação declarativa baseada em componente que ajuda-te a programar interfaces de utilizador eficientemente, sejam elas simples ou complexas.</i>'
        }
      ],
    };
  },
  methods: {
    handleNewContent() {
      if (event.key === "Enter" && !event.shiftKey) {
        event.preventDefault();

        if (!this.newContent.length || this.newContent.length < 3) {
          this.showAlert = {
            class: "alert-danger",
            message: "Digite alguma mensagem minimo 3 caracteres...",
          };
          return;
        }

        this.contents.push({
          title: this.newContent.slice(0, 32).concat("..."),
          body: this.newContent.replace(/\r\n|\n|\r/gm, '<br/>'),
          show: false,
        });

        this.newContent = "";
      }
    },
    handleShowContent(key) {
      this.contents[key].show = !this.contents[key].show;
    },
    handleDeleteContent(key) {
      this.contents = this.contents.filter((value, index) => index !== key);
    },
    handleTitleEdit() {
      if (event.ctrlKey && event.type === "click") {
        this.readonly = false;
      }

      if (event.key === "Enter") {
        if (event.target.value === event.target._value || event.target.value.length < 3) {
          if (event.target.value.length < 3) {
            this.showAlert = {
              class: "alert-danger",
              message: "Digite alguma mensagem minimo 3 caracteres...",
            };
          }
          return;
        }

        this.showAlert = [];
        this.contents[event.target.dataset.keyval].title = event.target.value;
        this.readonly = true;
      }
    },
  },
  watch: {
    newContent() {
      if (this.showAlert.message && this.newContent.length >= 3) {
        this.showAlert = [];
      }
    },
  },
};
</script>

<template>
  <div class="center">
    <h1>TODO LIST</h1>
    <span class="alert" v-show="showAlert.message" :class="showAlert.class">{{ showAlert.message }}</span>
    <textarea rows="5" v-model="newContent" @keypress="handleNewContent">{{ newContent }}</textarea>
    <small for="">Shift + Enter (Pula linhas)</small>
    <div v-show="contents.length" class="card">
      <h6>Pressione CRTL + Click no titulo para editar.</h6>

      <TodoItems @onTitleEdit="handleTitleEdit" @onShowContent="handleShowContent" @onDeleteContent="handleDeleteContent"
        :contents="contents" :readonly="readonly" />
    </div>
  </div>
</template>

<style scoped>
@import url("https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css");

* {
  font-family: "Arial", sans-serif;
}

.center {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 80%;
  margin-inline: auto;
  margin-block: 5rem;
}

.center h1 {
  margin-block-end: 2rem;
  text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.45);
}

.center textarea {
  margin-bottom: 0.4rem;
  margin-inline: 1rem;
  width: 35rem;
  resize: none;
  font-size: 16pt;
  padding: 0.7rem;
  border: 0.7px solid rgb(218, 218, 218);
  outline: none !important;
  border-radius: 0 !important;
}

.center textarea:focus,
.center textarea:active {
  border-radius: 0 !important;
  border: 0.7px solid rgb(84, 238, 110);
  box-shadow: 0px 0px 20px 0px rgb(84 238 110 / 20%);
}

.card {
  margin-block-start: 5vh;
  padding: 0.65rem;
  width: 35rem;
}

.alert {
  padding: 1rem;
  background: rgb(255, 255, 255);
  width: 34rem;
  margin: auto;
  margin-block-end: 1rem;
}

.alert-danger {
  background: rgb(236, 67, 67);
  color: #fff;
  box-shadow: 0px 0px 10px rgb(236, 67, 67);
}
</style>
