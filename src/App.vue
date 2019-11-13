<template>
  <div class="wrapper">
    <div class="wrapper-content"> 
        <section>
          <div class="container"> 
            
            <!-- first modal -->
              <button class="btn btnPrimary" 
                    @click='modalFirst=!modalFirst'>SHOW FIRST MODAL</button>    
              <modal
                v-show='modalFirst'
                title="First modal"
                @close='modalFirst=false'>
                <div slot="body">
                  <p> Lorem, ipsum dolor sit </p>
                  <button class="btn btnPrimary" @click="modalFirst=!modalFirst"> Well Done!</button>
                </div>
              </modal>

            <!-- second modal  -->
            <button class="btn btnPrimary" 
                    @click='modalSecond.show = !modalSecond.show'>Show modal with form</button>    
              <modal
                title="Modal with form"
                v-show="modalSecond.show"
                @close="modalSecond.show = false"
                >
                <div slot="body">
                  <form v-on:submit.prevent = "submitSecondForm">
                    <label>Name:
                      <input type="text" v-model="modalSecond.name" required>
                    </label>
                    <label>Email:
                      <input type="text" v-model="modalSecond.email" required>
                    </label>
                    <button class="btn btnPrimary">Submit!</button>
                  </form>
                </div>
              </modal>
            <!-- modal with Validate  -->
              <button class="btn btnPrimary" 
                    @click="modalValidate = !modalValidate">Show  with form + validate </button>
                    <modalValidate v-show="modalValidate" 
                                    @close="modalValidate = false"
                    ></modalValidate>    
          </div>
        </section>
      </div>
    </div>

</template>

<script>
import modal from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
export default {
  components: {
    modal, modalValidate

},
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: ''
      },
      modalValidate: false,
    }          
},
methods: {
  submitSecondForm() {
    console.log({
      name: this.modalSecond.name,
      email: this.modalSecond.email,
    })
    this.modalSecond.name = '' ;
    this.modalSecond.email = '' ;
    this.modalSecond.show = false;

  }
}
}
</script>