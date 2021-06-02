<template>
    <div>
        <form class="bg-info p-3 m-2" @submit.prevent="save" method="post">
            <div class="form-group">
                <label for="name">Name</label>
                <input v-model="info.name" type="text" class="form-control" id="name" placeholder="Enter Your Name">
            </div>
            <div class="form-group">
                <label for="age">Age</label>
                <input v-model="info.age" type="number" class="form-control" id="age" placeholder="Enter Your Age">
            </div>
            <button class="btn btn-success btn-block" type="submit">{{editMode ? "Update" : "Save"}}</button>
        </form>

        <div class="m-2">
            <table class="table table-bordered table-striped table-primary">
                <tr>
                    <th>SL</th>
                    <th>Name</th>
                    <th>Age</th>
                    <th>Action</th>
                </tr>
                <tr v-for="(list,index) in lists" :key="index">
                    <td>{{index+1}}</td>
                    <td>{{ list.name }} </td>
                    <td>{{ list.age }} </td>
                    <td>
                        <button @click="edit(list)" class="btn btn-sm btn-warning">Edit</button>
                        <button type="button" @click="del(list.id)"  class="btn btn-sm btn-danger">Delete</button>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    name: "Todo",
    data() {
        return{
            lists : [],
            info : {
                name : "",
                age : "",
            },
            editMode : false,
            userId : null,
        }
    },
    methods:{
        save(){
            try {
                var link;
                if(this.editMode){
                    link = "/user/"+this.userId;
                }else{
                    link = "/user"
                }
                axios.post(link,this.info)
                .then(res =>{
                    this.fetchAll();
                    this.info.name = "";
                    this.info.age = "";
                    this.editMode = false;
                    this.userId = null;
                })
            } catch (error) {
                console.log(error);
            }
        },
        del(id){
            try {
                axios.delete('/user/'+id)
                .then(res =>{
                    this.fetchAll();
                })
            } catch (error) {
                console.log(error);
            }
        },
        edit(data){
            this.info.name = data.name;
            this.info.age = data.age;
            this.editMode = true;
            this.userId = data.id;
        },
        fetchAll(){
            try {
                axios.get('/user')
                .then(res =>{
                    this.lists = res.data;
                })
            } catch (error) {
                console.log(error);
            }
        }
    },
    mounted(){
        this.fetchAll();
    }

}
</script>

<style scoped>
    form{
        border-radius: 5px;
        border : 1px solid blue;
        box-shadow : 2px 2px 20px 2px black;
    }
</style>
