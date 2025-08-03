<template>
  <div>
    <h1>Edit Word</h1>
    <word-form @createOrUpdate="createOrUpdate" :word="word"></word-form>
  </div>
</template>

<script>
import WordForm from '../components/WordForm.vue';
import { api } from '../helpers/helpers';

export default {
  name: 'edit',
  components: {
    'word-form': WordForm
  },
  data: function () {
    return {
      word: {
        english: '',
        german: ''
      }
    };
  },
  async mounted() {
    try {
      this.word = await api.getWord(this.$route.params.id);
    } catch (error) {
      this.flash('Error loading word!', 'error');
      console.error('Error loading word:', error);
    }
  },
  methods: {
    createOrUpdate: async function(word) {
      try {
        await api.updateWord(word);
        this.flash('Word updated successfully!', 'success');
        this.$router.push(`/words/${word._id}`);
      } catch (error) {
        this.flash('Error updating word!', 'error');
        console.error('Error updating word:', error);
      }
    } 
  }
};
</script>