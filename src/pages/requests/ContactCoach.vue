<template>
    <base-dialog title="Error" :show="(reqError !== null)" @close="handleRequestError">
        <p>{{ error }}</p>
    </base-dialog>
    <form @submit.prevent="submitForm">
        <div class="form-control">
            <label for="email">Your E-Mail</label>
            <input type="email" id="email" v-model.trim="email">

        </div>
        <div class="form-control">
            <label for="message">Your Message</label>
            <textarea rows="5" id="message" v-model.trim="message"></textarea>
        </div>
        <p class="errors" v-if="!isFormValid">Please fix your data and try again</p>
        <div class="actions">
            <base-button>Send Messsage</base-button>
        </div>
    </form>
</template>


<script>

export default {
    data() {
        return {
            reqError: null,
            email: '',
            message: '',
            isFormValid: true,
        }
    },
    methods: {
        handleRequestError() {
            this.reqError = null;
        },
        async submitForm() {
            this.reqError = null;
            this.isFormValid = (this.email !== '' && this.email.includes('@') && this.message !== '')

            if (!this.isFormValid) return;

            try {
                this.$store.dispatch('requests/contactCoach', {
                    email: this.email,
                    message: this.message,
                    coachId: this.$route.params['id']
                });

                this.$router.replace('/coaches')
            } catch (err) {
                this.reqError = null;
            }
        }
    }
}

</script>

<style lang="scss" scoped>
@use "../../scss/variables" as *;

form {
    margin: 1rem;
    border: 1px solid $clr-grey-30;
    border-radius: 12px;
    padding: 1rem;
}

.form-control {
    margin: 0.5rem 0;
}

label {
    font-weight: bold;
    margin-bottom: 0.5rem;
    display: block;
}

input,
textarea {
    display: block;
    width: 100%;
    font: inherit;
    border: 1px solid $clr-grey-30;
    padding: 0.15rem;

    
&:focus {
    border-color: $clr-blue-10;
    background-color: $clr-white-10;
    outline: none;
}
}
.errors {
    font-weight: bold;
    color: $clr-error;
}

.actions {
    text-align: center;
}
</style>