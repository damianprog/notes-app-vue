<template>
    <div class="main-container">
        <h1>Notes App Vue</h1>
        <note
                v-for="note in allNotes"
                :note-data="note"
                @save="onSave($event)"
        ></note>
    </div>
</template>

<script>
    import Note from "./Note";

    export default {
        name: "NotesList",

        components: {
            Note
        },

        data() {
            return {
                notes: [{
                    createdAt: Date.now().toString(),
                    text: " # Welcome to Vue notes!\n\n ### You can:\n\n " +
                        "- **Click plus sign above to create a new note**\n\n" +
                        "- **Click this note to edit**\n\n" +
                        "- **Click outside this note to save it **\n\n" +
                        "- **... and many more! **\n\n"
                }]
            }
        },

        computed: {
            allNotes() {
                return this.notes.sort();
            }
        },

        methods: {
            onSave(updatedNote) {
                this.notes = this.notes.filter(note => note.createdAt !== updatedNote.createdAt);
                this.notes.push({...updatedNote});

                window.localStorage.setItem("notes-app-vue", JSON.stringify(this.notes));
            }
        },

        mounted: function () {
            const storage = window.localStorage;
            const notes = JSON.parse(storage.getItem("notes-app-vue"));

            if (notes && notes.length > 0) {
                this.notes = [...notes];
            } else {
                storage.setItem("notes-app-vue", JSON.stringify([{
                    createdAt: Date.now().toString(),
                    text: "# Welcome to Vue notes!\n\nYou can:."
                }]));
            }
        }
    }
</script>

<style scoped>
    .main-container {
        text-align: center;
    }

</style>