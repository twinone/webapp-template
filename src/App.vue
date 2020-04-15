<template>
    <v-app>
        <v-app-bar
                app
                color="primary"
                dark
        >
            <v-app-bar-nav-icon @click.stop="drawer = !drawer"/>

            <v-toolbar-title class="pl-0">Title</v-toolbar-title>

        </v-app-bar>

        <v-navigation-drawer app clipped touchless v-model="drawer">
            <DrawerTreeListComponent :routes="routes"/>
        </v-navigation-drawer>


        <v-content>
            <router-view style="max-width: 50rem; margin:auto !important;"/>
        </v-content>
    </v-app>
</template>

<script>
    import DrawerTreeListComponent from './components/DrawerTreeListComponent'

    export default {
        name: 'App',

        components: {
            DrawerTreeListComponent,
        },
        data: () => ({
            drawer: false,
            routes: [],
        }),

        created() {
            this.routes = this.$router.options.routes
        },

        computed: {

            filteredRoutes() {
                // let rt = this.routes.filter(route => route.meta)
                let rt = this.routes.filter(route => route.meta && route.meta.showInNav && route.meta.showInNav() === true)
                return rt
            },
        },
    }
</script>
