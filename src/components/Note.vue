<template>
    <div class="note-container">
        <div class="top-bar">
            <span>{{creationDate}}</span>
            <div class="icons">
                <i class="fas fa-check"></i>
                <i class="fas fa-trash-alt"></i>
            </div>
        </div>
        <div class="markdown-display"
             @click="showEdit"
             v-show="!edit"
             v-html="compiledMarkdown"
        ></div>
        <textarea v-model="textInput"
                  ref="editArea"
                  @blur="saveInput"
                  v-show="edit"
        ></textarea>
    </div>
</template>

<script>
    export default {
        name: "Note",

        props: {
            noteData: {
                type: Object,
                default: () => {
                }
            }
        },

        data() {
            return {
                edit: false,
                textInput: this.noteData.text
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

                return new Date(Number(this.noteData.createdAt))
                    .toLocaleDateString("en-US", options);
            },
            compiledMarkdown: function () {
                return marked(this.noteData.text);
            }
        },

        methods: {
            showEdit() {
                this.edit = true;
                this.$nextTick(() => this.$refs.editArea.focus());
            },

            saveInput() {
                this.$emit("save", {...this.noteData, text: this.textInput});
                this.edit = false;
            }
        },
    }
</script>

<style scoped>
    .note-container {
        background-color: #f9f9f9;
        min-height: 400px;
        max-width: 560px;
        margin: auto;
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
        width: 560px;
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
    }

</style>