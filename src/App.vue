<template lang="pug">
  #app
    h1 Gestor De Tareas
    hr
    h3 Administrador: {{ name }}
    p.format-msg(v-show="!tasks.length") No Hay Tareas Creadas
    div.wi(v-show="tasks.length")
      ol
        li(v-for="(task,s)  in tasks") 
          span Tarea: {{ task.title }} Tiempo: {{ task.time }} s
          span.bc(@click="removeTask(s)") X
    div.form
      input(v-model="newTask.title" type="text" placeholder="Tarea")
      input(v-model="newTask.time" type="number")
      button(@click="addTask") Agregar Tarea
      button(@click='cancel') Cancelar
    span(style="font-weight:bold;") Tiempo Total: {{ totalTime }}
    div
      p By Fernando Soto  ©copyright 

</template>

<script>
export default {
  name: 'app',
  
  data () {
    return {
      name: 'Fernando Soto',
      tasks: [],
      newTask: { title: null, time: null },
      inputTitle: '',
      inputTime: 0
    }
  },

  created () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },

  computed: {

    totalTime () {
      let total = 0
      if (this.tasks === null) {
        return 0
      }
      else {
        this.tasks.forEach(taskTime => total +=parseInt(taskTime.time))
      }
      return total
    }

  },

  methods: {
    addTask: function() {
      if (!this.newTask.title || !this.newTask.time ) {
        return alert("Por Favor Digite El Nombre De La Tarea y Tiempo Emplado.")
      }
      this.tasks.push({ title: this.newTask.title, time: this.newTask.time })

      localStorage.setItem("tasks", JSON.stringify(this.tasks))
      
      this.newTask.title = ''
      this.newTask.time = 0
    },

    removeTask (start) {
      this.tasks.splice(start, 1)
      localStorage.setItem("tasks", JSON.stringify(this.tasks))
    },

    cancel () {
      this.newTask.title = ''
      this.newTask.time = 0
    }
  }

}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0 auto;
}

.format-msg {
  color: red;
}
.wi {
   margin: 0 auto;
   width: 40%;
}

.form {
  padding-bottom: 10px;
}

.bc {
  background-color: red;
  padding: 5px;
  border: 1px blue solid;
  margin-left: 10px;
}

ol {
  border: 2px solid goldenrod;
  border-radius: 5px;
  padding: 20px;
}

ol li {
  padding-bottom: 10px;
  padding-top: 10px; 
  // background: powderblue;
  text-align: left;
   margin-left: 35px;
}

</style>
