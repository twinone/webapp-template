<template>
    <v-list dense nav shaped>
        <v-list-item-group :class="{nested:nested}" active-class="router-link-exact-active">
            <div
                    v-for="(item, index) in filteredRoutes"
                    :key="index"
            >
                <div class="d-flex flex-column">

                    <v-list-item
                            @click.native="open(index)"
                            :to="item.children? '' :item.path" tag="div"

                    >
                        <v-list-item-action>
                            <div :class="{'rotating':item.meta.rotate}">
                                <v-icon>
                                    {{ item.meta.icon }}
                                </v-icon>
                            </div>
                        </v-list-item-action>
                        <v-list-item-content>
                            {{item.name}}
                        </v-list-item-content>
                        <v-list-item-action v-if="item.children">
                            <v-icon :class="{'rotate180':!!openChildren[index]}">mdi-chevron-down</v-icon>
                        </v-list-item-action>
                        <v-list-item-action class="ml-auto" v-if="item.meta.chip">
                            <v-chip small :color="item.meta.chip.color">{{item.meta.chip.text}}</v-chip>
                        </v-list-item-action>

                    </v-list-item>
                    <v-expand-transition>
                        <DrawerTreeListComponent v-if="item.children && !!openChildren[index]" :routes="item.children"
                                                 :nested="true"/>
                    </v-expand-transition>
                </div>
            </div>
        </v-list-item-group>
    </v-list>
</template>
<script>
    export default {
        name: 'DrawerTreeListComponent',
        props: {
            nested: {default: false},
            closeDrawer: {},
            routes: {},
        },
        data: function () {
            return {
                openChildren: {2: true},
            }
        },
        methods: {
            open(i) {
                this.$set(this.openChildren, i, !this.openChildren[i])
            },
        },

        mounted() {
            console.log(this.routes.length, 'len')
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
<style>

    .v-list-item__action {
        margin-right: 1.5em !important;
    }

    .has-children {
        margin-bottom: 0 !important;
    }

    .nested .v-list-item__action {
        margin-top: 0 !important;
        margin-bottom: 0 !important;
    }

    .v-list-item {
        text-decoration: none !important;
    }


    .nested {
        border-left: 1px solid #88888888;
        padding-left: 1em;
        margin-top: 0 !important;
        margin-bottom: 0 !important;
    }


</style>
