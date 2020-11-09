<template>
 <v-main class="list">
   <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

   <v-card>
     <v-card-title>
       <v-text-field
         v-model="search"
         append-icon="mdi-magnify"
         label="Search"
         single-line
         hide-details
       ></v-text-field>
       <v-spacer></v-spacer>
       <v-select
        v-model="select"
        :items="['All Priority','Penting','Biasa','Tidak penting']"
        label="Priority">
       </v-select>
       <v-spacer></v-spacer>
       <v-btn color="success" dark @click="dialog = true">
         Tambah
       </v-btn>
     </v-card-title>
     <v-data-table :headers="headers" :items="todos"  :search="search" :select="select">
       <template v-slot:[`item.actions`]="{ item }">
         <v-btn small class="mr-2" @click="dialogEdit=true,editItem(item)">
           edit 
         </v-btn>
         <v-btn small @click="deleteItem(item)">
           delete
         </v-btn>
       </template>
     </v-data-table>
   </v-card>

   <v-dialog v-model="dialog" persistent max-width="600px">
     <v-card>
       <v-card-title>
         <span class="headline">Form Todo</span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="formTodo.task"
             label="Task"
             required
           ></v-text-field>

           <v-select
             v-model="formTodo.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="formTodo.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>
         <v-btn color="blue darken-1" text @click="save">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>

   <v-dialog v-model="dialogEdit" persistent max-width="600px">
   <v-card>
       <v-card-title>
         <span class="headline">Edit Todo</span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="editedItem.task"
             label="Task"
             required
           ></v-text-field>

           <v-select
             v-model="editedItem.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="editedItem.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancelEdit">
           Cancel
         </v-btn>
         <v-btn color="blue darken-1" text @click="saveEdit">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>
   <v-dialog v-model="dialogDelete" max-width="400px">
          <v-card>
            <v-card-title class="headline">Yakin ingin menghapus?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red" text @click="noDelete">No</v-btn>
              <v-btn color="green" text @click="yesDelete">Yes</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
   
 </v-main>
</template>
<script>
export default {
 name: "List",
 data() {
    return {
     search: null,
     dialog: false,
     dialogEdit:false,
     dialogDelete: false,
     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority", value: "priority" },
       { text: "Note", value: "note" },
       { text: "Actions", value: "actions" },
     ],
     editedIndex:-1,
     editedItem:{
       task:"",
       priority:"",
       note:"",
     },
     todos: [
       {
         task: "Belajar PAW",
         priority: "Penting",
         note: "Mpdul 9",
       },
       {
         task: "Tidur",
         priority: "Biasa",
         note: "seperlunya",
       },
       {
         task: "Pergi",
         priority: "Tidak Penting",
         note: "tidak diperlukan",
       },
     ],
     formTodo: {
       task: null,
       priority: null,
       note: null,
     },
   };
 },
 computed:{
     select: function(select){
         return select.item
     }
 },
 methods: {
   save() {
     this.todos.push(this.formTodo);
     this.resetForm();
     this.dialog = false;
   },
   saveEdit(){
     if (this.editedIndex > -1) {
          Object.assign(this.todos[this.editedIndex],this.editedItem)
        } else {
          this.todos.push(this.editedItem)
        }
        this.dialogEdit=false
   },
   cancel() {
     this.resetForm();
     this.dialog = false;
   },
   cancelEdit(){
     this.resetForm();
     this.dialogEdit = false;
     this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.formTodo)
          this.editedIndex = -1
        })
   },
   resetForm() {
     this.formTodo = {
       task: null,
       priority: null,
       note: null,
     };
   },
   editItem(item){
     this.editedIndex=this.todos.indexOf(item);
     this.editedItem=Object.assign({},item);
     this.dialog=false;
   },
   deleteItem(item){
      this.editedIndex = this.todos.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete=true;
   },

   yesDelete(){
       this.todos.splice(this.editedIndex, 1);
       this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.formTodo)
          this.editedIndex = -1
       })
       this.dialogDelete=false
   },
   noDelete(){
       this.dialogDelete=false
   },
 },
    
};
</script>
