<template>
  <div class="container">
  <h2 class="text-center mt-5">My Vue Todo App</h2>
 
  <!-- Input field -->
  <div class="d-flex">
    <!--v-model="task" updates the value of the task property  -->
    <input v-model="task" type="text" class="form-control" placeholder="Enter Task Here">
    <button @click = "submitTask" class="btn btn-warning rounded-0">Submit</button>
  </div>
  
  <table class="table table-striped mt-5">
  <thead class="table-success">
    <tr> <!-- For loop for task[]-->
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody v-for="(task,index) in tasks" :key="index"><!-- Task table (Boot strap table)-->
    <tr>
      <td scope="row">
        <span :class="{'finsihed': task.status === 'finished'}">{{task.name}}</span>
        </td>
      <td style="width: 120px">
        <span @click="changeStatus(index)" class = "pointer">{{firstCharUpper(task.status)}}</span>
        </td>
      <td>
        <div class="text-center"  @click = "editTask(index)"> <!--Click event for delete icon. The index arguments identifies which task to edit-->
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center"  @click = "deleteTask(index)"><!--Click event for delete icon. The index arguments identifies which task should be deleted-->
          <span class="fa fa-trash"></span>
        </div>
        </td>
    </tr>
  </tbody>
</table> 
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){   
    return{
      // This will replace data entry
      // a for look is required to add this data to the table
      // capturing value from the input field
      task: '', //V model directive is required for data binding
      editedTask: null,  // this prevents adding new object to the array/table
      // initial value of null. The index is assigned in the editTask(index) function.

      availableStatuses: ['pending','in-progress', 'finished'], // Status values that will be assigned to the table for when user clicks

      tasks:[
        {
          name: 'Grocerry run',
          status: 'Pending'
        },
        {
          name: 'Carwash',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTask(){
      // Input field validation
      if(this.task.length === 0) return alert("Task field is empty!");

      if(this.editedTask === null){ // the null identifies the user did not click on the edit icon
        this.tasks.push({ //pushing a new object in to the tasks[] array
          name: this.task,
          status: 'in-progress' //in-progress is the default value assigned to new tasks
        });
      }else{
        //when user click on the edit icon
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      // Clear input field after submitting a task
      this.task ='';
    },

    deleteTask(index){
      this.tasks.splice(index, 1); //the seconds arguement identifies how many items should be deleted
    },

    editTask(index){
      // task proper declared in the task.push method   
      this.task = this.tasks[index].name; //tasks[] array that holds all the objects and displays them to the table
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status); // This will find the value of the current index and increment it by 1
                                                                              // If the value of the index is greater than array's length it will be set to 0
    if(++newIndex > 2) newIndex = 0;
    this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
     }
  }
};
</script>
<style scoped>
/* Creating a cursor pointer */
.pointer {
  cursor: pointer;
}
/* Css decoration to cross out the completed task */
.finsihed{
  text-decoration: line-through;
  color: red;
}
</style>

