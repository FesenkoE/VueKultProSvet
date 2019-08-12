<template>
    <div class="container">
        <h1 class="text-center">{{ title }}</h1>
        <span>Total notes: <b>{{  note.count }}</b></span>
        <div class="form">
            <div class="form-group">
                <label>Note title</label>
                <input
                        type="text"
                        class="form-control"
                        v-model="note.title">
                <span class="text-danger" v-if="validationMessage.title">{{ validationMessage.title }}</span>
            </div>

            <div class="form-group">
                <label>Note text</label>
                <input
                        type="text"
                        class="form-control"
                        v-model="note.text">
                <span class="text-danger">{{ validationMessage.text }}</span>
            </div>
            <div class="form-group">
                <label>Note email</label>
                <input
                        type="text"
                        class="form-control"
                        v-model="note.email">
                <span class="text-danger">{{ validationMessage.email }}</span>
            </div>
            <button
                    class="btn btn-primary"
                    :disabled="isButtonDisabled"
                    @click="addNote">Submit
            </button>
        </div>
        <div class="col-sm-12">
            <div class="col-sm-4 note" v-for="(note, index) in notes">
                <div class="card mt-2">
                    <div class="card-body">
                        <button class="close" @click="removeNote(index)">x</button>
                        <h4 class="card-title">{{ note.title }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted">
                            {{ note.date }}
                        </h6>
                        <p class="card-text">{{ note.text }}</p>
                        <p class="text-info"> {{ note.email }} </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Notes",
        data() {
            return {
                title: 'Notes',
                note: {
                    title: '',
                    text: '',
                    email: '',
                    date: new Date(Date.now()).toLocaleString(),
                    count: 0,
                },
                notes: [
                    {
                        title: 'Visited Hawaii',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'some@gmail.com',
                        date: new Date(Date.now()).toLocaleString(),
                    },
                    {
                        title: 'Visited London',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'some@gmail.com',
                        date: new Date(Date.now()).toLocaleString(),
                    },
                ],
                isButtonDisabled: true,
                validationMessage: {
                    title: '',
                    text: '',
                    email: ''
                }
            }
        },
        methods: {
            addNote() {
                let {text, title, email} = this.note;
                this.notes.push({
                    text,
                    title,
                    email,
                    date: new Date(Date.now()).toLocaleString(),
                });
                this.note.title = '';
                this.note.text = '';
                this.note.email = '';
                this.note.count = this.notes.length;
            },
            removeNote(index) {
                this.notes.splice(index, 1);
                this.note.count = this.notes.length;
            }
        },
        created() {
            this.note.count = this.notes.length;
        },
        watch: {
            note: {
                handler: function (val) {
                    if (val.title !== '') {
                        this.validationMessage.title = '';
                    } else {
                        this.validationMessage.title = 'not null and min 3 characters';
                    }


                },
                deep: true
            }
        }
    }
</script>