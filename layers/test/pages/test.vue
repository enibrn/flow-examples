<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title>Vuetify Test Page</v-toolbar-title>
      <v-spacer />
      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      app
      temporary
    >
      <v-list-item>
        <v-list-item-avatar>
          <v-img src="https://randomuser.me/api/portraits/men/78.jpg" />
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title>John Doe</v-list-item-title>
          <v-list-item-subtitle>Admin</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider />

      <v-list dense>
        <v-list-item
          v-for="item in menuItems"
          :key="item.title"
          :to="item.to"
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container fluid>
        <h1 class="mb-5">Vuetify Components Test</h1>

        <!-- Cards Section -->
        <h2 class="mb-3">Cards</h2>
        <v-row>
          <v-col
            v-for="n in 3"
            :key="n"
            cols="12"
            md="4"
          >
            <v-card
              class="mx-auto"
              max-width="400"
            >
              <v-img
                height="200"
                src="https://picsum.photos/400/200?random={{n}}"
              />
              <v-card-title>Card Title {{ n }}</v-card-title>
              <v-card-text>
                This is example card content. Lorem ipsum dolor sit amet, consectetur adipiscing elit.
              </v-card-text>
              <v-card-actions>
                <v-btn
                  text
                  color="primary"
                >
                  Action
                </v-btn>
                <v-spacer />
                <v-btn icon>
                  <v-icon>mdi-heart</v-icon>
                </v-btn>
                <v-btn icon>
                  <v-icon>mdi-share-variant</v-icon>
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>

        <!-- Form Elements -->
        <h2 class="my-5">Form Elements</h2>
        <v-form ref="form">
          <v-row>
            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="formData.name"
                label="Name"
                required
                outlined
              />
            </v-col>
            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="formData.email"
                label="Email"
                type="email"
                required
                outlined
              />
            </v-col>
            <v-col cols="12">
              <v-select
                v-model="formData.country"
                :items="countries"
                label="Country"
                outlined
              />
            </v-col>
            <v-col cols="12">
              <v-textarea
                v-model="formData.message"
                label="Message"
                outlined
              />
            </v-col>
            <v-col cols="12">
              <v-checkbox
                v-model="formData.newsletter"
                label="Subscribe to newsletter"
              />
            </v-col>
            <v-col cols="12">
              <v-btn
                color="primary"
                @click="submitForm"
              >Submit</v-btn>
              <v-btn
                class="ml-2"
                text
                @click="resetForm"
              >Reset</v-btn>
            </v-col>
          </v-row>
        </v-form>

        <!-- Data Table -->
        <h2 class="my-5">Data Table</h2>
        <v-data-table
          :headers="headers"
          :items="desserts"
          :items-per-page="5"
          class="elevation-1"
        />

        <!-- Alert and Dialog Demo -->
        <h2 class="my-5">Dialog and Alerts</h2>
        <v-row>
          <v-col cols="6">
            <v-btn
              color="primary"
              @click="dialog = true"
            >
              Open Dialog
            </v-btn>
          </v-col>
          <v-col cols="6">
            <v-btn
              color="error"
              @click="showAlert"
            >
              Show Alert
            </v-btn>
          </v-col>
        </v-row>

        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <v-card>
            <v-card-title>
              This is a Dialog
            </v-card-title>
            <v-card-text>
              Click outside or press the escape key to close this dialog.
            </v-card-text>
            <v-card-actions>
              <v-spacer />
              <v-btn
                color="primary"
                text
                @click="dialog = false"
              >
                Close
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-snackbar
          v-model="alert"
          :timeout="3000"
          color="error"
        >
          This is an alert message
          <template #action="{ attrs }">
            <v-btn
              text
              v-bind="attrs"
              @click="alert = false"
            >Close</v-btn>
          </template>
        </v-snackbar>
      </v-container>
    </v-main>

    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }} - Vuetify Test Page</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'TestPage',
  data() {
    return {
      drawer: false,
      dialog: false,
      alert: false,
      menuItems: [
        { title: 'Dashboard', icon: 'mdi-view-dashboard', to: '/' },
        { title: 'Settings', icon: 'mdi-cog', to: '/settings' },
        { title: 'Profile', icon: 'mdi-account', to: '/profile' }
      ],
      formData: {
        name: '',
        email: '',
        country: null,
        message: '',
        newsletter: false
      },
      countries: ['USA', 'Canada', 'UK', 'Australia', 'Germany', 'France', 'Italy', 'Japan'],
      headers: [
        { text: 'Dessert (100g serving)', value: 'name' },
        { text: 'Calories', value: 'calories' },
        { text: 'Fat (g)', value: 'fat' },
        { text: 'Carbs (g)', value: 'carbs' },
        { text: 'Protein (g)', value: 'protein' }
      ],
      desserts: [
        {
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0
        },
        {
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3
        },
        {
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0
        },
        {
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3
        },
        {
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9
        }
      ]
    }
  },
  methods: {
    submitForm() {
      // Form submission logic would go here
      alert('Form submitted')
    },
    resetForm() {
      this.$refs.form.reset()
    },
    showAlert() {
      this.alert = true
    }
  }
}
</script>

<style scoped>
h1,
h2 {
  color: #333;
}

.v-card {
  margin-bottom: 20px;
}
</style>