<template>
    <modal
        title="Modal with form"
        @close="$emit('close'), reset()"
        >
        <div slot="body">
            <form @submit.prevent = "onSubmit">
                <!-- name -->
                <div class="form-item" :class="{ errorInput: $v.name.$error }">
                <label>Name:
                    <p class="errorText" v-if="!$v.name.required">Filed is required!</p>
                    <p class="errorText" v-if="!$v.name.minLength">Very small text. Need {{ $v.name.$params.minLength.min}}!!!</p>
                    <input 
                        v-model="name"
                        :class="{ error: $v.name.$error }"
                        @change="$v.name.$touch()"
                        > 
                </label>
                </div>
                <!-- email -->
                <div class="form-item" :class="{ errorInput: $v.email.$error }">
                <label>Email:
                    <p class="errorText" v-if="!$v.email.required">Filed is required!</p>
                    <p class="errorText" v-if="!$v.email.email">Not correct email!!!</p>
                    <input 
                        v-model="email"
                        :class="{ error: $v.email.$error }"
                        @change="$v.email.$touch()"
                        > 
                </label>
                </div>
                <!-- password -->
                <div class="form-item" :class="{ errorInput: $v.password.$error }">
                <label>Password:
                    <p class="errorText" v-if="!$v.password.required">Filed is required!</p>
                    <p class="errorText" v-if="!$v.password.minLength">Password min {{ $v.password.$params.minLength.min }} symbol</p>
                    <input type='password'
                        v-model="password"
                        :class="{ error: $v.password.$error }"
                        @change="$v.password.$touch()"
                        > 
                        <span style="font-size: 10px"><i> Please, repeat password {{ checkPass }} </i></span>
                    <input type="password"
                        v-model="passRepeat"
                        :class="{ error: $v.passRepeat.$error, correctPas: correctPas }"
                        @change="$v.passRepeat.$touch()"
                    >
                </label>
                </div>
                <button class="btn btnPrimary">Submit!</button>
            </form>
        </div>
    </modal>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue'
export default {
    components: {
        modal
    },
    data () {
        return {
            name: '',
            email: '',
            password: '',
            passRepeat: '',
            phrasePas: '',
            correctPas: false,
        }
    },
    validations: {
        name: {
            required,
            minLength: minLength(4),
        },
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(4),
        },
        passRepeat: {
            required
        }
    },
    methods: {
        onSubmit() {
        this.$v.$touch();
        if(!this.$v.$invalid & this.passRepeat === this.password) {
            const user = {
                name: this.name,
                email: this.email,
                password: this.password
            }
            console.log(user);
            //DONE!
            this.reset();
            this.$emit('close');
        }
        },
        reset() {
            this.name = '';
            this.email = '';
            this.password = '';
            this.passRepeat = '';
            this.correctPas = false;
            this.$v.$reset();
        }
    },
    computed: {
        checkPass(){
            if (this.password !=''){
                this.correctPas = (this.password === this.passRepeat) ? true: false;
            }
        }
    }
}
</script>

<style lang="scss">
    .form-item .errorText {
        display: none;
        margin-bottom: 8px;
        font-size: 13.5px;
        color: #de4040;
    }
    .form-item {
        &.errorInput .errorText {
        display: block;
    }
    }

    input.error {
        border-color:#de4040;
    }
    .correctPas{
        border-color: green;
    }
    
</style>