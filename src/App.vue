<script setup>
import NewCandidateComponent from './components/NewCandidateComponent.vue'
</script>

<script>
  export default {
    data() {
      return {
          candidateList: [],
          isHide: true,
          fieldValue: ''
      }
    },
    methods: {
      addCandidate(candidate) {
        this.candidateList.push(candidate)
      },
      removeCandidate() {
        const value = event.target.getAttribute('data-candidate')
        this.candidateList = this.candidateList.filter(candidate => candidate!= value)
        if (this.candidateList.length == 0) {
          this.isHide = true
        }
      },
      editCandidate() {
        this.isHide = !this.isHide
        var container = event.target.closest('.chosen-candidate');
        var name = container.querySelector('p').innerText
        document.querySelector('.div-edit').setAttribute('data-edit', name)
        document.querySelector('.div-edit input').setAttribute('placeholder', name)
      },
      onInputChange() {
        this.fieldValue = event.target.value;
      },
      submitEdit() {
        var value = document.querySelector('.div-edit').getAttribute('data-edit')
        var index = this.candidateList.indexOf(value)
        if (index !== - 1) {
          this.candidateList[index] = this.fieldValue
        }
        this.fieldValue = ''
        this.isHide = !this.isHide
      }
    }
  }
</script>

<template>
  <header>
  </header>
  <main>
    <NewCandidateComponent @choose-candidate="addCandidate"/>
    <div class="chosen-list">
      <h3> Candidate List ({{ candidateList.length }})</h3>
      <div class="div-edit" data-edit="" :class="{hide: isHide}">
        <p>Edit name</p>
        <input @change="onInputChange" type="text" v-model="text">
        <button @click="submitEdit">Update</button>
      </div>
      <ul>
        <li class="chosen-candidate" v-for="candidate in candidateList">
            <p @click="enableEdit">{{ candidate }}</p>
            <div>
              <button @click="editCandidate">Edit</button>
              <button @click="removeCandidate" :data-candidate="candidate">Remove</button>
            </div>
        </li>
      </ul>
    </div>
  </main>
</template>



<style global>
    .hide {
      display: none !important;
    }
    .chosen-list {
        text-align: center;
    }
    ul {
        list-style-type: none;
    }
    .chosen-candidate {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid black;
    }
</style>
