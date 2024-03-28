<template>
  <PageLayout>
    <section class="p-16">
      <PersonForm v-model="form" />
      <SimpleButton 
        class ="person-page__btn" 
        type="primary" 
        @click="() => createPerson()"
      >
        Сохранить
      </SimpleButton>
      <SimpleButton 
        class ="person-page__btn" 
        type="danger" 
        @click="() => cancel()"
      >
        Отмена
      </SimpleButton>
    </section>
  </PageLayout>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import PageLayout from '../parts/PageLayout.vue'
import PersonForm from '../forms/PersonForm.vue'
import { emptyPerson } from '@/services/person'
import SimpleButton from '../ui/SimpleButton.vue'

export default {
  name: 'CreatePersonPage',
  components: {
    PageLayout,
    PersonForm,
    SimpleButton
  },
  data () {
    return {
      form: emptyPerson()
    }
  },
  computed: {
    ...mapGetters('settings', [
      'getMode'
    ])
  },
  methods: {
    ...mapActions('persons', [
      'addPerson'
    ]),
    createPerson () {
      for (let i = 0; i < this.form.militaries.length; i++) {
        if (this.form.militaries[i].length === 0) {
          this.$notify({
            title: 'Ошибка.',
            message: 'Форма (Военная служба ' + (i + 1) + ') пустая!'
          });
        return;
        }
      }
      for (let i = 0; i < this.form.weddings.length; i++) {
        if (this.form.weddings[i].length === 0) {
          this.$notify({
            title: 'Ошибка.',
            message: 'Форма (Свадьба ' + (i + 1) + ') пустая!'
          });
        return;
        }
      }
      for (let i = 0; i < this.form.educations.length; i++) {
        if (this.form.educations[i].length === 0) {
          this.$notify({
            title: 'Ошибка.',
            message: 'Форма (Образование ' + (i + 1) + ') пустая!'
          });
        return;
        }
      }
      for (let i = 0; i < this.form.works.length; i++) {
        if (this.form.works[i].length === 0) {
          this.$notify({
            title: 'Ошибка.',
            message: 'Форма (Работа ' + (i + 1) + ') пустая!'
          });
        return;
        }
      }
      for (let i = 0; i < this.form.children.length; i++) {
        if (this.form.children[i].length === 0) {
          this.$notify({
            title: 'Ошибка.',
            message: 'Форма (Ребёнок ' + (i + 1) + ') пустая!'
          });
        return;
        }
      }
      this.addPerson(this.form)
        .then((person) => {
          this.$router.push({ name: this.$routes.PERSON, params: { id: person.id } })
        })
    },
    cancel () {
      this.goBack()
    },
    goBack () {
      this.$router.go(-1)
    }
  },
  mounted () {
    if (this.getMode === 'user') {
      this.$router.push({ name: this.$routes.HOME })
    }
  }
}
</script>

<style scoped lang="less">
.person-page {
  &__btn {
    margin-top: 10px;
    margin-right: 10px;
    margin-bottom: 20px;
  }
}
</style>
 