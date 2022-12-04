<template>

    <h1>Edit Project</h1>
    <form @submit.prevent="handleSubmit">
        <label>Title</label>
        <input type="text" v-model="title" required>
        <label>Details</label>
        <textarea v-model="details" required></textarea>
        <div class="completed-cbox">
            <input type="checkbox" v-model="complete">
            <label>Completed?</label>
        </div>
        <button>Update Project</button>

    </form>
</template>



<script>
    export default {
        props: [
            'id',
        ],
        data(){
            return{
                title: '',
                details: '',
                complete: false,
            }
        },
        mounted(){
            fetch('http://localhost:3000/projects/' + this.id)
            .then((res) => res.json())
            .then( data => {
                this.title = data.title
                this.details = data.details
                this.complete = data.complete
            })
        },
        methods: {
            handleSubmit(){
                // console.log(this.title, this.details);
                let updatedProject = {
                    title: this.title,
                    details: this.details,
                    complete: this.complete,
                }
                // console.log(newProject);

                fetch('http://localhost:3000/projects/' + this.id, {
                    method: 'PATCH',
                    headers: { 'Content-type': 'application/json' },
                    body: JSON.stringify(updatedProject)
                })
                .then(()=>{
                    this.$router.push('/')
                })
                .catch(err => console.log(err.message))
            }
        }
    }
</script>



<style>
    .completed-cbox {
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }
    input[type="checkbox"] {
        width: auto;
        margin: 0 10px;
        position: relative;
        top: 4px;
    }
</style>