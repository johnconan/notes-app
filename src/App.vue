<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">
          <p class="heading"> {{ title }} </p>
          <message v-if="message" :message="message" />

          <!-- new note -->
          <newNote :priority="priority" :note="note" @addNote="addNote" />

          <div class="note-header" style="margin: 36px 0;">
            <h1 class="title"> {{ title }} </h1>
            <search :value="search" placeholder="Find notes" @search="search = $event"/>
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>
          <!-- note list -->
          <notes 
            :editFields="editFields" 
            :notify="note.notify" 
            :notes="filterNotes"
            :grid="grid" 
            @edit="editNote"
            @cancelEdit="cancelEdit"
            @remove="removeNote"
            @saveEdit="saveEdit"
          />

        </div>
      </section>

    </div>
  </div>
</template>

<script>
import message from '@/components/Message.vue';
import notes from '@/components/Notes.vue';
import newNote from '@/components/NewNote.vue';
import search from '@/components/Search.vue';

export default {
  components: {
    message,
    notes,
    newNote,
    search
  },
  data() {
    return {
      title: 'Notes App',
      message: null,
      grid: true,
      search: '',
      editFields: {
        editTitle: null,
        editDescr: null
      },
      note: {
        title: '',
        descr: '',
        notify: 'standart',
        edit: false
      },
      priority: [ 
        {
          text: 'No important', value: 'standart'
        },
        {
          text: 'Important', value: 'average'
        },
        {
          text: 'Very important', value: 'important'
        }
      ],
      notes: [
        {
          title: 'First note',
          descr: 'First description',
          date: new Date(Date.now()).toLocaleString(),
          notify: 'standart',
          edit: false
        },
        {
          title: 'Second note',
          descr: 'Second description',
          date: new Date(Date.now()).toLocaleString(),
          notify: 'average',
          edit: false
        },
        {
          title: 'Third note',
          descr: 'Third description',
          date: new Date(Date.now()).toLocaleString(),
          notify: 'important',
          edit: false
        }
      ]
    }
  },
  methods: {
    addNote() {
      let { title, descr, notify, edit } = this.note;
      if (title === '') {
        this.message = `title don't empty!`;
        return false;
      }
      this.notes.push({
        title,
        descr,
        notify,
        edit,
        date: new Date(Date.now()).toLocaleString()
      });
      this.note.title = '';
      this.note.descr = '';
      this.message = null;
      this.note.notify = 'standart';
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    editNote(index) {
      this.editFields.editTitle = this.notes[index].title;
      this.editFields.editDescr = this.notes[index].descr;
      this.notes[index].edit = true;
    },
    cancelEdit(index) {
      this.editFields.editTitle = null;
      this.editFields.editDescr = null;
      this.notes[index].edit = false;
    },
    saveEdit(index) {
      this.notes[index].title = this.editFields.editTitle;
      this.notes[index].descr = this.editFields.editDescr;
      this.notes[index].date = new Date(Date.now()).toLocaleString();
      this.cancelEdit(index);
    }
  },
  computed: {
    filterNotes() {
      let arr = this.notes,
          search = this.search;
      if (!search) return arr;
      search = search.trim().toLowerCase();
      arr = arr.filter((item) => {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });
      return arr;
    }
  }
}
</script>