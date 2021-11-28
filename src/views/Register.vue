<template>
        <div class="auth-page ng-scope">
        <div class="container page">
            <div class="row">

                <div class="col-md-6 offset-md-3 col-xs-12">
                    <h1 class="text-xs-center" >Sign up</h1>
                    <p class="text-xs-center">
                        <router-link :to="{name: 'login'}">Have an account?</router-link>
                    </p>
                    <mcv-validation-errors v-if="validationErrors" :validation-errors="validationErrors" />
<!--                    <list-errors class="ng-isolate-scope"><ul class="error-messages ng-hide">-->
<!--                        &lt;!&ndash; ngRepeat: (field, errors) in $ctrl.errors &ndash;&gt;-->
<!--                    </ul>-->
<!--                    </list-errors>-->

                    <form class="ng-pristine ng-valid ng-valid-email" @submit.prevent="onSubmit">
                        <fieldset>

                            <fieldset class="form-group">
                                <input class="form-control form-control-lg ng-pristine ng-untouched ng-valid ng-empty" type="text" placeholder="Username" v-model="username">
                            </fieldset>

                            <fieldset class="form-group">
                                <input class="form-control form-control-lg ng-pristine ng-untouched ng-valid ng-empty ng-valid-email" type="email" placeholder="Email" v-model="email">
                            </fieldset>

                            <fieldset class="form-group">
                                <input class="form-control form-control-lg ng-pristine ng-untouched ng-valid ng-empty" type="password" placeholder="Password" v-model="password">
                            </fieldset>

                            <button class="btn btn-lg btn-primary pull-xs-right ng-binding" type="submit" :disabled="isSubmitting">Sign up</button>

                        </fieldset>
                    </form>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    import McvValidationErrors from "@/components/ValidationErrors";
    import {actionTypes} from "@/store/modules/auth";

    export default {
        name: "McvRegister",
        components: {McvValidationErrors},
        data () {
            return {
                email: '',
                password: '',
                username: '',
            }
        },
        computed: {
            ...mapState({
                isSubmitting: state => state.auth.isSubmitting,
                validationErrors: state => state.auth.validationErrors
            })
        },
        methods: {
            onSubmit() {
                console.log('submited form')
                this.$store.dispatch(actionTypes.register, {
                    email: this.email,
                    username: this.username,
                    password: this.password,
                }).then(user => {
                    console.log('successfully register user', user )
                    this.$router.push({name: 'globalFeed'})
                })
            },
        }
    }
</script>

<style scoped>

</style>