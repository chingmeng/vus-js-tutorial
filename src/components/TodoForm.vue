<template>
  <div class="container">

    <form @submit="submit">
      <p>
        <label>Title</label><br/>
        <input type="text" v-model="title" name="title" placeholder="Add Todo...">
        <br/>
        <span v-if="this.title === ''" style="font-size: 10pt; color:red;">Title cannot be blank</span>
      </p>
      <p>
        <label>Detail</label><br/>
        <textarea v-model="detail" name="detail" rows="4" cols="50" placeholder="Write your detail here..."></textarea>
      </p>
      <p>
        <label>Completed</label>&nbsp;&nbsp;
        <input type="checkbox" v-model="completed" name="completed">
      </p>
      <p>
        <br/>
        <input type="submit" value="Submit" class="btn">
      </p>
    </form>
  </div>

</template>
 }
}

<script>
import axios from 'axios';

export default {
  name: "AddTodo",
  data() {
    return {
      title: ''
    }
  },
  methods: {
    submit(e) {
      e.preventDefault();

      if (this.title === "") {
        return
      }

      const { id, title, detail, completed } = {
        id: this.id,
        title: this.title,
        detail: this.detail,
        completed: this.completed
      };

      if (this.id === undefined) {
        axios.post('http://127.0.0.1:8001/todo/api/', {
          title, detail, completed
        })
        .then(res => {
          this.$router.go(-1);
        })
        .catch(err => console.log(err));
      } else {
        axios.post('http://127.0.0.1:8001/todo/api/', {
          id, title, detail, completed
        })
        .then(res => {
          this.$router.go(-1);
        })
        .catch(err => console.log(err));
      }
    }
  },
  created() {
    if (this.$route.params !== undefined) {
      this.id = this.$route.params.id
      this.title = this.$route.params.title
      this.detail = this.$route.params.detail
      this.completed = this.$route.params.completed === "true"
    } else {
      this.id = undefined
    } 
  }
}
</script>

<style scoped>
  .container {
    display: flex;
    align-items: stretch;
    flex-direction: row;
    width: 100%;
  }

</style>