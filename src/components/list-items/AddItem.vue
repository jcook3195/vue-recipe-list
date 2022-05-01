<template>
    <base-modal v-if="inputIsInvalid" title="Invalid Input" @closeModal="confirmError">
        <template #default>
            <p>At least one input value is invalid.</p>
            <p>Please check that all inputs have a value entered.</p>
        </template>
        <template #actions>
            <base-button @click="confirmError">Okay</base-button>
        </template>
    </base-modal>
    <base-card>
        <form @submit.prevent="submitData">
            <div class="form-control">
                <label for="title">Title</label>
                <input id="title" name="title" type="text" ref="titleInput">
            </div>
            <div class="form-control">
                <label for="quantity">Quantity</label>                
                <input id="quantity" name="quantity" type="number" ref="quantityInput">
            </div>
            <div class="form-control">
                <label for="src">Image Src</label>
                <input id="src" name="src" type="url" ref="srcInput">
            </div>
            <div>
                <base-button type="submit">Add Item</base-button>
            </div>
        </form>
    </base-card>
</template>

<script>
export default {
    inject: ['addItem'],
    data() {
        return {
            inputIsInvalid: false,
        };
    },
    methods: {
       submitData() {
           const eneteredTitle = this.$refs.titleInput.value;
           const enteredQuantity = this.$refs.quantityInput.value;
           const enteredUrl = this.$refs.srcInput.value;

           if (eneteredTitle.trim() === '' || enteredQuantity.trim() === '' || enteredUrl.trim() === '') {
               this.inputIsInvalid = true;

               return;
           }

            this.$refs.titleInput.value = '';
            this.$refs.quantityInput.value = '';
            this.$refs.srcInput.value = '';

            this.addItem(eneteredTitle, enteredQuantity, enteredUrl);
       },
       confirmError() {
           this.inputIsInvalid = false;
       }
    }
}
</script>

<style scoped>
    label {
        font-weight: bold;
        display: block;
        margin-bottom: 0.5rem;
    }

    input,
    textarea {
        display: block;
        width: 100%;
        font: inherit;
        padding: 0.15rem;
        border: 1px solid #ccc;
    }

    input:focus,
    textarea:focus {
        outline: none;
        border-color: #3a0061;
        background-color: #f7ebff;
    }

    .form-control {
        margin: 1rem 0;
    }
</style>