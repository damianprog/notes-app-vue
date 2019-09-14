<template>
    <div class="note-container">
        <div class="top-bar">
            <span>{{creationDate}}</span>
            <div class="icons">
                <i :class="actionIcon"></i>
                <i @click="deleteNote" class="fas fa-trash-alt"></i>
            </div>
        </div>
        <div class="markdown-display"
             @click="showEdit"
             v-show="!edit"
             v-html="compiledMarkdown"
        ></div>
        <textarea v-model="textInput"
                  ref="editArea"
                  @blur="saveNote"
                  v-show="edit"
        ></textarea>
    </div>
</template>

<script>
    export default {
        name: "Note",

        props: {
            note: {
                type: Object,
                default: () => {
                }
            }
        },

        data() {
            return {
                edit: false,
                textInput: this.note.text
            }
        },

        computed: {
            creationDate() {
                const options = {
                    weekday: 'long',
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric',
                    hour: "2-digit",
                    minute: '2-digit'
                };

                return new Date(Number(this.note.createdAt))
                    .toLocaleDateString("en-US", options);
            },
            compiledMarkdown: function () {
                return marked(this.note.text);
            },
            actionIcon() {
                return this.edit ? "fas fa-edit" : "fas fa-check";
            }
        },

        methods: {
            showEdit() {
                this.edit = true;
                this.$nextTick(() => this.$refs.editArea.focus());
            },

            saveNote() {
                this.$emit("save", {...this.note, text: this.textInput});
                this.edit = false;
            },

            deleteNote() {
                if (confirm("Are you sure you want to delete this note?")) {
                    this.$emit("delete",{...this.note})
                }
            }
        },

        watch: {
            note(data) {
                this.textInput = data.text;
            }
        }

    }
</script>

<style scoped>
    .note-container {
        background-color: #f9f9f9;
        min-height: 400px;
        max-width: 560px;
        margin: 0 auto 40px auto;
        text-align: left;
        box-shadow: 3px 3px 10px #000;
        border-radius: 3px;
    }

    .top-bar {
        height: 43px;
        width: 100%;
        background-color: #eee;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 10px;
        border-radius: 3px;
    }

    .top-bar span {
        color: #4ca2cd;
        font-size: 14px;
        font-style: italic;
    }

    .top-bar .icons i:first-child {
        padding-right: 15px;
    }

    .icons {
        font-size: 25px;
    }

    .note-container textarea {
        /*max-width: 560px;*/
        width: 100%;
        min-height: 357px;
        display: block;
        padding: 10px;
        resize: none;
    }

    .markdown-display {
        padding: 0 10px 10px 10px;
        min-height: 347px;
    }

    .fa-edit {
        color: #ff9100;
    }

    .fa-check {
        color: #64dd17;
    }

    .fa-trash-alt {
        color: #dd2c00;
        cursor: pointer;
    }

</style>