<template>
  <v-app id="inspire">
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="8">
            <v-card class="elevation-12">
              <v-window v-model="step">
                <v-window-item :value="1">
                  <v-row>
                    <v-col cols="12" md="8">
                      <v-card-text class="mt-12">
                        <h1
                          class="text-center display-2 "
                        >Sign in </h1>
                       
                        <v-form>
                          <v-text-field
                            label="Email"
                            name="Email"
                            prepend-icon="email"
                            type="text"
                            color="#4287f5"
                            v-model="email"
                          />

                          <v-text-field
                            id="password"
                            label="Password"
                            name="password"
                            prepend-icon="lock"
                            type="password"
                            color="#4287f5"
                            v-model="password"
                          />
                        </v-form>
                      </v-card-text>
                      <div class="text-center mt-3 mb-4">
                        <v-btn  rounded color="#4287f5" v-model="signIn" dark >SIGN IN</v-btn>
                      </div>
                    </v-col>
                    <v-col cols="12" md="4" class="blue darken-1">
                      <v-card-text class="white--text ">
                        <img src="../assets/Scanlogo.png" style="width:360px;height:350px;">
                      </v-card-text>
                     
                    </v-col>
                  </v-row>
                </v-window-item>
              </v-window>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script setup>
    import { ref } from 'vue'
    import {getAuth, signInWithEmailAndPassword} from "firebase/auth";
    import {  useRouter } from 'vue-router' // import router
    const email = ref('')
    const password = ref('')
    const errMsg = ref() // ERROR MESSAGE
    const router = useRouter() // get a reference to our vue router
    const signIn = () => { // we also renamed this method
      signInWithEmailAndPassword(getAuth(), email.value, password.value) // THIS LINE CHANGED
        .then(() => {
          console.log('Successfully logged in!');
          router.push('/dashboard') // redirect to the feed
        })
        .catch(error => {
          switch (error.code) {
            case 'auth/invalid-email':
                errMsg.value = 'Invalid email'
                break
            case 'auth/user-not-found':
                errMsg.value = 'No account with that email was found'
                break
            case 'auth/wrong-password':
                errMsg.value = 'Incorrect password'
                break
            default:
                errMsg.value = 'Email or password was incorrect'
                break
          }
        });
    }
</script>
