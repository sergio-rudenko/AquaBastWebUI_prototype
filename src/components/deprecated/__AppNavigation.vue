<template>
    <span>
        <v-navigation-drawer v-model="drawer" temporary app>
            <!-- <v-subheader>Пользователь:</v-subheader> -->
            <v-list-item>
                <v-list-item-avatar>
                    <v-img src="../assets/araragi.jpg"></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                    <v-list-item-title>{{ userName }}</v-list-item-title>
                    <v-list-item-subtitle>{{ userPhone }}</v-list-item-subtitle>
                    <v-list-item-subtitle>{{ userMail }}</v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense rounded>
                <template v-for="(item, i) in items">
                    <v-row v-if="item.heading" :key="i" align="center">
                        <v-col cols="6">
                            <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
                        </v-col>
                        <!-- <v-col cols="6" class="text-right">
                            <v-btn small text>edit</v-btn>
                        </v-col>-->
                    </v-row>
                    <v-divider v-else-if="item.divider" :key="i" dark class="my-4" />
                    <v-list-item v-else :key="i" :to="item.path" link>
                        <v-list-item-action>
                            <v-icon>{{ item.icon }}</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title>{{ item.title }}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </template>
                <!-- <v-list-item
                    v-for="item in items"
                    :key="item.title"
                    :to="item.path"
                    link
                >
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>-->
            </v-list>
            <template v-slot:append>
                <v-list-item>
                    <div class="pa-2">
                        <v-switch
                            v-model="darkMode"
                            prepend-icon="mdi-weather-night"
                            label="Ночной режим"
                            hide-details
                            dense
                        ></v-switch>
                    </div>
                </v-list-item>
                <div class="pa-2">
                    <v-btn block rounded>Logout</v-btn>
                </div>
            </template>
        </v-navigation-drawer>

        <v-app-bar app>
            <!-- Hamburger -->
            <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

            <v-toolbar-title>{{ appTitle }}</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
                <v-icon>mdi-help-circle</v-icon>
            </v-btn>
        </v-app-bar>
    </span>
</template>

<script>
export default {
    name: 'AppNavigation',

    mounted() {
        this.darkMode = localStorage.getItem('darkMode') == 'true';
        this.$vuetify.theme.dark = this.darkMode;
    },

    watch: {
        darkMode(value) {
            localStorage.setItem('darkMode', value);
            this.$vuetify.theme.dark = value;
        }
    },

    data() {
        return {
            appTitle: 'AquaBast',
            darkMode: false,
            drawer: false,
            items: [
                // { heading: 'Меню' },
                {
                    title: 'Вход в систему',
                    icon: 'mdi-login-variant',
                    path: 'login'
                },
                {
                    title: 'Список устройств',
                    icon: 'mdi-view-list',
                    path: '/'
                },
                { divider: true },
                {
                    title: 'Панель управления',
                    icon: 'mdi-view-dashboard',
                    path: 'dashboard'
                },
                {
                    title: 'Настройки',
                    icon: 'mdi-settings',
                    path: 'settings'
                },
                { divider: true },
                {
                    title: 'О программе',
                    icon: 'mdi-message-text-outline',
                    path: 'about'
                }
            ]
        };
    },

    computed: {
        userName() {
            return this.$store.state.credentials.username;
        },

        userPhone() {
            let p = this.$store.state.credentials.phone;
            let c = ('' + p).replace(/\D/g, '');
            let a = c.match(/^(\d{2})(\d{3})(\d{3})(\d{4})$/);
            if (!a) {
                a = c.match(/^(\d{1})(\d{3})(\d{3})(\d{4})$/);
            }
            if (a) {
                return '+' + a[1] + ' (' + a[2] + ') ' + a[3] + '-' + a[4];
            }
            return null;
        },

        userMail() {
            return this.$store.state.credentials.mail;
        }
    }
};
</script>

<style scoped></style>
