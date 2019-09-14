<template>
    <div class="main-container">
        <div class="header-actions">
            <h1>Notes App Vue</h1>
            <i @click="createNote" class="fas fa-plus"></i>
        </div>
        <notes-list-note
                v-for="note in sortedNotes"
                :note="note"
                @save="onSave($event)"
                @delete="onDelete($event)"
        ></notes-list-note>
    </div>
</template>

<script>
    import NotesListNote from "./NotesListNote";

    export default {
        name: "NotesList",

        components: {
            NotesListNote
        },

        data() {
            return {
                notes: []
            }
        },

        computed: {
            sortedNotes() {
                return this.notes.sort((a,b) => {
                    return a.createdAt - b.createdAt;
                }).reverse();
            }
        },

        methods: {
            onSave(note) {
                this.notes = this.notes.filter(n => n.createdAt !== note.createdAt);
                this.notes.push({...note});

                this.updateStorage();
            },
            createNote() {
                this.notes.push({createdAt: Date.now().toString(),text: ""});

                this.updateStorage();
            },
            onDelete(note) {
                this.notes = this.notes.filter(n => n.createdAt !== note.createdAt);

                this.updateStorage();
            },
            updateStorage() {
                window.localStorage.setItem("notes-app-vue", JSON.stringify(this.notes));
            }
        },

        mounted: function () {
            const storage = window.localStorage;
            const notes = JSON.parse(storage.getItem("notes-app-vue"));

            if (notes && notes.length > 0) {
                this.notes = [...notes];
            } else {
                this.notes = [{
                    createdAt: Date.now().toString(),
                    text: " # Welcome to Vue notes!\n\n ### You can:\n\n " +
                        "- **Click plus sign above to create a new note**\n\n" +
                        "- **Click note to edit**\n\n" +
                        "- **Click outside note to save it **\n\n" +
                        "- **Click bin icon to remove note **\n\n" +
                        "- **... and many more! **\n\n"
                }];
                this.updateStorage();
            }
        }
    }
</script>

<style scoped>
    .main-container {
        text-align: center;
        margin: 0 15px;
    }

    h1 {
        margin: 0;
    }

    .header-actions {
        display:flex;
        justify-content: space-between;
        max-width: 560px;
        margin: 20px auto;
    }

    .fa-plus {
        color: #880e4f;
        font-size: 35px;
        cursor: pointer;
    }

</style>