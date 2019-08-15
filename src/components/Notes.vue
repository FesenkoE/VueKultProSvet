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
                <span class="text-danger">{{ validationMessage['title'] }}</span>
            </div>

            <div class="form-group">
                <label>Note text</label>
                <input
                        type="text"
                        class="form-control"
                        v-model="note.text">
                <span class="text-danger">{{ validationMessage['text'] }}</span>
            </div>
            <div class="form-group">
                <label>Note email</label>
                <input
                        type="text"
                        class="form-control"
                        v-model="note.email">
                <span class="text-danger">{{ validationMessage['email'] }}</span>
            </div>
            <button
                    class="btn btn-primary"
                    :disabled="isButtonDisabled"
                    @click="addNote">Submit
            </button>
        </div>
        <div class="col-sm-12 mt-2">
            <div class="row">
                <button class="btn btn-primary mr-1" @click="sortByDate">Sort by Date</button>
                <button class="btn btn-primary" @click="sortByTitle">Sort by Title</button>
            </div>
        </div>
        <div class="col-sm-12">
            <div class="col-sm-4 note" v-for="(note, index) in notes">
                <div class="card mt-2">
                    <div class="card-body">
                        <button class="close" @click="removeNote(index)">x</button>
                        <button class="close mr-1" @click="copyNote(index)"><i class="far fa-copy"></i></button>
                        <h4 class="card-title">{{ note.title }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted">
                            {{ note.date }}
                        </h6>
                        <p class="card-text">{{ note.text }}</p>
                        <p class="text-info"> {{ note.email }} </p>
                        <span class="close">{{ note.description }}</span>
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
                        date: '13.08.2019',
                    },
                    {
                        title: 'Visited London',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'some@gmail.com',
                        date: '11.08.2019',
                    },
                ],
                isButtonDisabled: true,
                validationMessage: [],
                titleError: '',
                textError: '',
                emailError: '',
            }
        },
        methods: {
            addNote() {
                let {text, title, email} = this.note;
                this.notes.push({
                    text,
                    title,
                    email,
                    date: this.formatDate(),
                });
                this.note.title = '';
                this.note.text = '';
                this.note.email = '';
                this.note.count = this.notes.length;
            },
            removeNote(index) {
                this.notes.splice(index, 1);
                this.note.count = this.notes.length;
            },
            copyNote(index) {
                this.notes.push({
                    text: this.notes[index].text,
                    title: this.notes[index].title,
                    email: this.notes[index].email,
                    date: this.formatDate(),
                    description: 'copy'
                });
            },
            sortByTitle() {
                this.notes.sort(function(a, b) {
                    if(a.title < b.title) { return -1; }
                    if(a.title > b.title) { return 1; }
                    return 0;
                });
            },
            sortByDate() {
                this.notes.sort(function(a, b) {
                    if(a.date < b.date) { return -1; }
                    if(a.date > b.date) { return 1; }
                    return 0;
                });
            },
            formatDate() {
                let now = new Date();
                let month = now.getMonth() < 10 ? '0' + now.getMonth() : now.getMonth();
                return now.getDate() + '.' + month + '.' + now.getFullYear();
            }
        },
        created() {
            this.note.count = this.notes.length;
        },
        watch: {
            note: {
                handler: function (val) {
                    if (val.title.length < 4 || val.title.charAt(0) !== val.title.charAt(0).toUpperCase()) {
                        this.validationMessage['title'] = 'Title has min 3 characters and first capital letter';
                    } else {
                        this.validationMessage['title'] = '';
                    }

                    if (val.text.split(/ +(?:\S)/).length < 3) {
                        this.validationMessage['text'] = 'Text has min 3 words';
                    } else {
                        this.validationMessage['text'] = '';
                    }

                    let pattern = /^[a-z0-9_-]+@[a-z0-9-]+\.[a-z]{2,6}$/i;

                    if (val.email.search(pattern) !== 0) {
                        this.validationMessage['email'] = 'Invalid email';
                    } else {
                        this.validationMessage['email'] = '';
                    }

                    this.isButtonDisabled = !((!this.validationMessage['title']) &&
                        (!this.validationMessage['text']) &&
                        (!this.validationMessage['email']));
                },
                deep: true
            }
        }
    }
</script>
