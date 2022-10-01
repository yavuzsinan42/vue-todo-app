
<template>
     <div class="row">
        <div class="col-md-12 border d-flex justify-content-between p-3 ">
          <p>{{todo.text}}</p>
          <button class="btn btn-danger" @click="$emit('deleteTodo', todo.id)">Sil</button>
        </div>
      </div>
</template>
<script>
  export default {
    props: ["todo"]
  }
</script>