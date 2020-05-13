<template>
  <div id="app">

    <div class="menu">
      <label for="name">Имя</label>
      <input v-model="cur_note.first_name" class="form-control"/>

      <label for="name">Фамилия</label>
      <input v-model="cur_note.second_name" class="form-control"/>
        
      <label for="surname">Отчество</label>
      <input v-model="cur_note.third_name" class="form-control"/>


      <label for="phone">Телефон</label>
      <input v-model="cur_note.phone" class="form-control"/>

      <label for="phone">Избранный</label>
      <input type="checkbox" v-model="cur_note.favorite" />

      <button class="btn btn-outline-success" v-on:click="toggleSave()">Добавить</button>
      </div>

    <div class="delete">

      <label for="name">id</label>
      <input type="text" id="deleteid" class="form-control" v-model="deleteid"/>
      <button class="btn btn-outline-warning" @click="onDelete()">Удалить элемент по id </button>

    </div>

    <div class="change">
      <label for="name">id</label>
      <input type="text" id="changeid" class="form-control" v-model="changeid"/>

      <label for="name">Имя</label>
      <input type="text" id="changename" class="form-control" v-model="changename"/>

      <label for="surname">Фамилия</label>
      <input type="text" id="changesurname" class="form-control" v-model="changesurname"/>

      <label for="surname">Отчество</label>
      <input type="text" id="changethirdname" class="form-control" v-model="changethirdname"/>

      <label for="phone">Телефон</label>
      <input type="text" id="changephone" class="form-control" v-model="changephone"/>

      <label for="phone">Избранный</label>
      <input v-model="changefav" type="checkbox" name="a" value="1934">
        <button class="btn btn-outline-info" @click="onChange()">Изменить элемент по id</button>
      </div>

    <div class="book">
      <h2> Записная книжка : </h2>
      <div class="radio">
        <p>
          Сортировка по имени<input value="name" type="radio" v-model="sort" v-on:click="toggleSort('name')" />
        </p>
        <p>
          Сортировка по фамилии<input value="sec_name" type="radio" v-model="sort" v-on:click="toggleSort('sec_name')"/>
        </p>
      </div>

      <li v-for="(item, index) in notes" v-bind:key="index">
        {{item.id}} {{item.first_name}} {{item.second_name}} {{item.third_name}}  {{item.phone}} <span v-if="item.favorite"> ★</span>
      </li>
      
    </div>
    
   


  </div>
</template>

<script>


  export default {

  name: 'App',
  data() {
  return{
  sort: "name",
  cur_note: {
  first_name: "",
  second_name: "",
  third_name: "",
  phone: "",
  favorite: false
  },
  notes: [],
  editing: -1,
  cur_id: -1,

  deleteid:0,
  changeid:0,
  changename:'',
  changesurname:'',
  changethirdname:'',
  changephone:'',
  changefav: false,


  };
  },
  components: {},


   
    methods: {
           
            async toggleSave() {
                if (this.cur_note.phone !== "") {
                    if (this.editing === -1) {
                        this.notes.push({
                            first_name: this.cur_note.first_name,
                            second_name: this.cur_note.second_name,
                            third_name: this.cur_note.third_name,
                            phone: this.cur_note.phone,
                            favorite: this.cur_note.favorite
                        });
                        try{
                            await this.$http.post('http://localhost:3000/notes', this.cur_note);
                            this.updateData();
                        }catch(e){
                            console.log(e);
                        }
                    } else {
                        this.notes[this.editing] = {
                            id: this.cur_note.id,
                            first_name: this.cur_note.first_name,
                            second_name: this.cur_note.second_name,
                            third_name: this.cur_note.third_name,
                            phone: this.cur_note.phone,
                            favorite: this.cur_note.favorite
                        };
                        try{
                            console.log(this.notes[this.editing]);
                            await this.$http.put('http://localhost:3000/notes/'+ this.cur_id, this.notes[this.editing]);
                            this.updateData();
                        }catch(e){
                            console.log(e);
                        }
                        this.editing = -1;
                    }
                    if (this.sort === "name")
                        this.notes.sort((a, b) => a.first_name < b.first_name ? 1 : -1);
                    else if (this.sort === "sec_name")
                        this.notes.sort((a, b) => a.second_name < b.second_name ? 1 : -1);
                    this.notes.sort((a, b) => a.favorite < b.favorite ? 1 : -1);
                    this.cur_note.first_name = "";
                    this.cur_note.second_name = "";
                    this.cur_note.third_name = "";
                    this.cur_note.phone = "";
                    this.cur_note.favorite = "";
                } else {
                    alert("Запись не может быть пустой");
                }
            },
            
                      
            async onChange(){
                let note = {
                    id: this.changeid,
                    first_name: this.changename,
                    second_name: this.changesurname,
                    third_name: this.changethirdname,
                    phone: this.changephone,
                    favorite: this.changefav
                };
                try {
                    await this.$http.put('http://localhost:3000/notes' + '/' + this.changeid , note);
                    this.updateData();
                }catch (err) {
                    console.error(err);
                }
            },
                        
            async onDelete(){             
                try {
                    await this.$http.delete('http://localhost:3000/notes' + '/' + this.deleteid);
                    this.updateData();
                }catch (err) {
                    console.error(err);
                }
            },
                      
            toggleSort (value) {
                if (value === "name")
                    this.notes.sort((a, b) => a.first_name < b.first_name ? 1 : -1);
                else if (value === "sec_name")
                    this.notes.sort((a, b) => a.second_name < b.second_name ? 1 : -1);
                this.notes.sort((a, b) => a.favorite < b.favorite ? 1 : -1);
            },
            
                  
            updateData() {
                try {
                    this.toggleSort(this.sort);
                    this.$http.get('http://localhost:3000/notes').then((res) => res.json()).then((res) => (this.notes = res));
                    this.toggleSort(this.sort);
                } catch (e) {
                    console.error(e);
                }
                },
            },
            created() {
                this.toggleSort(this.sort);
                this.updateData();
            },
            
            mounted() {
                console.log('hello');
                this.sort = 'name';
                this.sort = 'sec_name';
                this.sort = 'name';
                setTimeout(function(){
                    //this.toggleSort('name');
                }, 2000);
                
            }
            
    }

</script>