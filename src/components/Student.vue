<template>
    <div>
        <b-button v-b-modal.modal-prevent-closing>Add</b-button>


        <b-modal id="modal-prevent-closing" ref="modal" title="Submit Your Name" @show="resetModal" @hidden="resetModal"
            @ok="handleOk">
            <form ref="form" @submit.stop.prevent="handleSubmit">
                <b-form-group label="Name">
                    <b-form-input v-model="student.name"></b-form-input>
                </b-form-group>

                <b-form-group label="Age">
                    <b-form-input v-model="student.age"></b-form-input>
                </b-form-group>

                <b-form-group label="Phone">
                    <b-form-input v-model="student.phone"></b-form-input>
                </b-form-group>

                <b-form-group label="Adress">
                    <b-form-input v-model="student.adress"></b-form-input>
                </b-form-group>
            </form>
        </b-modal>
    </div>
</template>
  
<script>
export default {
    props: {
        itemEdit: {
            type: Object,
            default: null
        }
    },
    watch: {
        itemEdit() {
            if (this.itemEdit) {
                this.student = Object.assign({}, this.itemEdit)
            } else {
                this.student = {

                }
            }
        }
    },
    data() {
        return {
            student: {
                id: Math.floor(Math.random() * 1000),
                name: "",
                age: "",
                phone: "",
                adress: ""
            },
            name: '',
            nameState: null,
            submittedNames: []
        }
    },

    methods: {
        checkFormValidity() {
            const valid = this.$refs.form.checkValidity()
            this.nameState = valid
            return valid
        },
        resetModal() {
            this.name = ''
            this.nameState = null
        },
        handleOk(bvModalEvent) {
            this.$emit("save", this.student)
            console.log(this.student);
            // Prevent modal from closing
            bvModalEvent.preventDefault()
            // Trigger submit handler
            this.handleSubmit()
        },
        handleSubmit() {
            // Exit when the form isn't valid
            if (!this.checkFormValidity()) {
                return
            }
            // Push the name to submitted names
            this.submittedNames.push(this.name)
            // Hide the modal manually
            this.$nextTick(() => {
                this.$bvModal.hide('modal-prevent-closing')
            })
        }
    }
}
</script>