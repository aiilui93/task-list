<template>
  <div class="container pt-5">
    <div class="card">
      <h1>{{ title }}</h1>

      <div class="form-control">
          <input 
            type="text" 
            name="inputValueName"
            v-bind:placeholder="placeholderStringName" 
            v-bind:value="inputValueName"
            v-on:input="inputChangeHandler"
            v-on:keypress.enter="addTask"
          />
          <input 
            type="date" 
            name="inputValueTime"
            v-bind:placeholder="placeholderStringTime" 
            v-bind:value="inputValueTime"
            v-on:input="inputChangeHandler"
            v-on:keypress.enter="addTask"
          />
          <input 
            type="text" 
            name="inputValueSpend"
            v-bind:placeholder="placeholderStringSpend" 
            v-bind:value="inputValueSpend"
            v-on:input="inputChangeHandler"
            v-on:keypress.enter="addTask"
          />
      </div>
      

      <h3 :class="{'hidden': hasInput}">Ваша новая задача:</h3>

      <h2>{{inputValueName}} {{inputValueTime}} {{inputValueSpend}}</h2>

      <button class="btn" @click="addTask">Добавить</button>
      <hr/>
      
      <div class="task__list" v-if="notes.length !== 0" >
          <div class="task__item" :class="{ finished: note.done }" v-for="(note, index) in notes" v-bind:key="note" >
              <div class="idx">{{ index + 1 }}</div> 
              <div class="name">{{ note.name }}</div> 
              <div class="time">{{ note.time }}</div> 
              <div class="spend">{{ note.spend }}</div> 
              <button v-show="note.done == false" class="btn done" @click="note.done = true; this.saveNotes()">Сделано</button>
              <button v-show="note.done == true" class="btn undone" @click="note.done = false; this.saveNotes()">Вернуть</button>
              <button class="btn danger" @click="removeTask(index, $event)">Удалить</button>
          </div>
      </div>

    </div>
  </div>
</template>

<script>


export default {
  name: "App",
  data() {
    return {
      placeholderStringName: 'Что вам надо сделать?',
      placeholderStringTime: 'Когда и во сколько?',
      placeholderStringSpend: 'Сколько времени потратите?',
      title: 'Список дел на сегодня',
      inputValueName: '',
      inputValueTime: '',
      inputValueSpend: '',
      hasInput: true,
      notes: []
    }
  },
  mounted() {
    if(localStorage.getItem('notes')){
      try {
        this.notes = JSON.parse(localStorage.getItem('notes'));
      } catch(e) {
        localStorage.removeItem('notes');
      }
      
    }
  },
  methods: {
    inputChangeHandler(event){
      console.log(event.target.name)
      this[event.target.name] = event.target.value
      this.hasInput = false
    },
    addTask(event){
      console.log(event)
      this.notes.push( {
        name: this.inputValueName,
        time: this.inputValueTime,
        spend: this.inputValueSpend,
        done: false
      })
      this.saveNotes();
    },
    saveNotes(){
      const parsedNotes = JSON.stringify(this.notes)
      localStorage.setItem('notes', parsedNotes)
    },
    removeTask(index, event){
        this.notes.splice(index, 1);
       this.saveNotes();
       console.log('removeTask: ', index, event)
    },
    clearCurrentNote() {
      this.inputValueName = ''
      this.inputValueTime = ''
      this.inputValueSpend = ''
      this.hasInput = true
    }
  }

}

</script>


<style src="./assets/app.css"></style>
