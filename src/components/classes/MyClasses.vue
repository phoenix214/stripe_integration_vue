<template lang="html">
    <div>
        <!-- BANNER -->
        <div class="banner bg--stainless jagged jagged--fog">
            <div class="banner__content container container--fw">
                <div class="wrapper">
                    <div class="wrapper__inner">
                        <h1 class="ts--display">My Classes</h1>
                        <p class="ts--body is--secondary fontSize--l">View a list of all your in progress and completed classes.</p>
                    </div>
                    <!-- BROWSE CLASSES -->
                    <!-- NOTE: This button is only displayed if the user is enrolled in classes -->
                    <div class="wrapper__inner align--right">
                        <button v-if="classesInProgress.length > 0" class="btn btn--cta" @click="openClasses">Browse Classes</button>
                    </div>
                    <!-- /BROWSE CLASSES -->
                </div>
            </div>
        </div>
        <!-- /BANNER -->
        <!-- CLASS LIST -->
        <div class="content__section row container container--fw">

            <!-- EMPTY STATE -->
            <!--
            NOTE: Display only if the user hasn't enrolled in any classes.
        -->
            <div class="well is--empty align--center padding--xxl border--medium" v-if="classesInProgress.length == 0">
                <span class="ts--title">You haven't enrolled in any classes yet!</span>
                <div class="divider divider--s" style="margin-left:auto; margin-right:auto;"></div>
                <button class="btn btn--cta" @click="openClasses">Browse Classes</button>
            </div>
            <!-- /EMPTY STATE -->

            <!-- /CLASS LIST -->
            <purchased v-if="masterClasses.length > 0" />
            <inprogress v-if="classesInProgress.length > 0" />
            <completedClasses v-if="completedClasses.length > 0" />

        </div>

    </div>

</template>

<script>
    import { mapGetters } from 'vuex';
    import { Class, User } from '../../api';
    import CompletedClasses from './sections/CompletedClasses.vue'
    import PurchasedClasses from './sections/PurchasedClasses.vue'
    import InProgress from './sections/InProgressClasses.vue'

    export default {
        components: {
            completedClasses: CompletedClasses,
            inprogress: InProgress,
            purchased: PurchasedClasses
        },
        computed: {
            ...mapGetters([
                'user', 'classesInProgress', 'completedClasses', 'masterClasses'
            ]),
        },
        methods: {
            openClasses() {
                this.$router.push({ name: 'classes' });
            }
        },
        mounted() {
            //update in-progress, completed, and master classes now.
            Class.inProgress(this);
            Class.completed(this);
            Class.masterClasses(this);
        }
    }

</script>

<style lang="css">

</style>