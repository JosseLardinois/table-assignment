<template>
    <div class="popup">
        <div class="popup-inner">
            <slot />
            <label for="editTitle"></label><h1 id="editTitle" name="editTitle">Edit</h1>
            <label for="students">Choose a record to edit:</label>
            <br />
            <label for="id">Id</label>
            <input type="number" name="id" id="id">
            <br>
            <label for="name">Name</label>
            <input type="text" name="name" id="name">
            <br>
            <label for="school">School</label>
            <input type="text" name="school" id="school">
            <br>
            <div>
                <button @click='edit(studentData); TogglePopup(); '>Edit</button>

                <button @click='TogglePopup(); '>Close</button>
            </div>

        </div>
    </div>
</template>
<script>
    //importing bootstrap 
    import "bootstrap/dist/css/bootstrap.min.css";
    export default {
        name: 'App',
        mounted() {
            this.SetPlaceholder(this.currentRow);
        },
        methods: {
            edit(studentData) {
                const _id = document.getElementById("id").value;
                const _name = document.getElementById("name").value;
                const _school = document.getElementById("school").value;
                for (const obj of studentData) {
                    if (obj.ID == _id) {
                        obj.Name = _name;
                        obj.School = _school;
                        break;
                    }
                }
                return studentData;
            },
            SetPlaceholder(currentRow) {
                //alert(currentRow[0]);

                //Set Placeholders
                document.getElementById("id").placeholder = currentRow[0];
                document.getElementById("name").placeholder = currentRow[1];
                document.getElementById("school").placeholder = currentRow[2];
            }
        },
        props:{
            studentData: {
                type: Array,
            },
            fields: {
                type: Array,
            },
            TogglePopup: {

            },
            currentRow: {
                type: Array,
            }

        }
    }
    

</script>
<style>
    .popup{
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index:99;
        background-color: rgba(0,0,0,0.2);
        display: flex;
        align-items:center;
        justify-content: center;
    }
    .popup-inner{
        background: #FFF;
        padding: 32px;
    }
</style>