<script>
export default {
  name: 'TodoItems',
  props: {
    contents: Object,
    readonly: Boolean
  },
  emits: ['onTitleEdit', 'onShowContent', 'onDeleteContent'],
  methods: {
    filterHtmlTags(text) {

      const allowedTags = ['<br/>', '<b>', '</b>', '<strong>', '<u>', '<i>', '<a>'];

      const regex = new RegExp(`(${allowedTags.join('|')})`, 'g');

      const filteredText = text.replace(/<\/?[^>]+(>|$)/g, (match) => {
        return allowedTags.includes(match) ? match : '';
      });

      return filteredText;
    }
  }
}
</script>

<template>
  <ul v-for="(content, key) in contents">
    <li :data-content="key">
      <div class="item">
        <input @click.ctrl="$emit('onTitleEdit')" @keypress.enter="$emit('onTitleEdit', event)" :data-keyVal="key"
          :value="filterHtmlTags(content.title)" :readonly="readonly" />
        <button @click="$emit('onShowContent', key)"><i class='bx'
            :class="!content.show ? 'bxs-chevron-right' : 'bxs-chevron-up'"></i></button>
      </div>
      <p class="content" v-show="content.show">
        <hr>
      <div v-html="filterHtmlTags(content.body)"></div>
      <button @click="$emit('onDeleteContent', key)"><i class='bx bx-trash-alt'></i> Deletar</button>
      </p>
    </li>
  </ul>
</template>
  
<style scoped>
ul {
  padding: 0;
  list-style: none;
}

li {
  border: .7px solid rgb(218, 218, 218);
  padding: .7rem;
  margin-block: .5rem;
  transition: ease-in .3s;
  background: #fff;
}

li:hover {
  border: .7px solid rgb(84, 238, 110);
  box-shadow: 0px 0px 20px 0px rgb(84 238 110 / 20%);
}

li>.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

li .content {
  display: block;
}

.item input {
  padding: 0;
  border: 0;
  outline: none;
  border-left: .20rem solid rgb(84, 238, 110);
  width: 100%;
  color: #898989;
  padding-inline-start: .20rem;
}

.item input[readonly] {
  padding: none !important;
  border: none !important;
  outline: none;
}

.item button {
  border: none;
  background: none;
  font-size: 14pt;
  color: rgb(84, 238, 110);
}

.content button {
  border: 0;
  background: #ff6e6e;
  border-bottom: 3px solid #df5858;
  display: block;
  margin: auto;
  padding: .3rem 1rem;
  color: #fff;
  cursor: pointer;
}

p.content hr {
  box-shadow: none;
  margin-block: 1rem;
  border-style: solid;
  border-color: #ededed !important;
}
</style>