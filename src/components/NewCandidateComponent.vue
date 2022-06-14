<script>
    export default {
        data() {
            return {
                candidate: 
                    { title: '', first: '', last: '', age: '', img: ''},
                errors: []
            }
        },
        methods: {
            async getNewCandidate() {
                await axios.get('https://randomuser.me/api/')
                .then(response => {
                    const result = response.data.results[0]
                    this.candidate.title = result.name.title
                    this.candidate.first = result.name.first
                    this.candidate.last = result.name.last
                    this.candidate.img = result.picture.large
                    this.candidate.age = result.dob.age
                })
                .catch(e => {
                    this.errors.push(e)
                })
            },
            onBtnClick() {
                this.$emit('choose-candidate', this.getCandidateFullName)
                this.getNewCandidate()

            },
            onNextBtnClick() {
                this.getNewCandidate()
            }
        },
        mounted() {
            this.getNewCandidate()
        },
        computed: {
            getCandidateFullName() {
                return this.candidate.title + '. ' + this.candidate.first + ' ' + this.candidate.last
            }
        }
    }
</script>

<template>
      <div class="candidate">
        <img class="candidate-img" :src="candidate.img" alt="">
        <h3 class="candidate-name">
          {{ getCandidateFullName }}
        </h3>
        <p class="candidate-age">
          Age: {{ candidate.age }}
        </p>
        <button @click="onBtnClick">
          Choose this candidate
        </button>
      </div>
</template>

<style>
  .candidate {
    text-align: center;
  }
  .candidate-img {
    border-radius: 50%;
  }
  .chosen-list {
    text-align: center;
  }
</style>