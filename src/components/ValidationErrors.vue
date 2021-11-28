<template>
    <ul class="errorMessages">
        <li v-for="errorMessage in errorMessages" :key="errorMessage">{{ errorMessage }}</li>
    </ul>
</template>

<script>
    export default {
        name: "McvValidationError",
        props: {
            validationErrors: {
                type: Object,
                required: true,
            }
        },
        computed: {
            errorMessages() {
                console.log('errors', this.validationErrors)
                return Object.keys(this.validationErrors).map(name => {
                    const messages = this.validationErrors[name].join(', ')
                    return `${name} ${messages}`
                })
            }
        }
    }
</script>

<style scoped>

</style>