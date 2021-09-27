<template>
  <div id="app">
    <!-- <p>{{ message }}</p> -->
    <div class="form">
      <div class="form-group">
        <input v-model="title" placeholder="title" class="form-control">
      </div>
      <div class="form-group">
        <input v-model="description" placeholder="description" class="form-control">
      </div>
      <button @click="addMemo">メモを追加</button>
    </div>
    <ul class="flex">
      <li v-for="memo in memos" :key="memo.id" class="card">
        <div class="card-body">
          <div class="card-title">
            {{ memo.title }}
          </div>
          {{ memo.description }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      memos: "memos",
      title: '',
      description: '',
    }
  },
  mounted () {
    this.setMemo();
  },
  methods: {
    setMemo: function () {
      axios.get('/api/memos')
      .then(responce => (
        this.memos = responce.data
      ))
    },
    addMemo: function () {
      axios.post('/api/memos', {
        title: this.title,
        description: this.description
      })
      .then(responce => (
        this.setMemo()
      ));
    }
  }
}
</script>

<style lang="scss" scoped>
.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 32px;
  &-group {
    margin-bottom: 1rem;
  }
  &-control {
    width: 600px;
    min-height: 24px;
    padding: 4px 8px;
    border: 1px solid #ced4da;
    font-size: 1rem;
  }
}

button {
  width: 200px;
}

.flex {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.card {
  width: 238px;
  margin: 16px;
  border: 1px solid #ced4da;
  border-radius: .25rem;
  list-style: none;
  &-body {
    padding: 1.25rem;
  }
  &-title {
    margin-bottom: .75rem;
    font-weight: 600;
  }
}
</style>
