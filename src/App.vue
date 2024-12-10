<script setup lang="ts">
import { AmplifyButton, Authenticator } from '@aws-amplify/ui-vue';
import {
  associateWebAuthnCredential,
  signIn,
  type SignInInput,
} from 'aws-amplify/auth';

const services = {
  async handleSignIn(input: SignInInput) {
    return await signIn({
      username: input.username,
      options: {
        authFlowType: 'USER_AUTH',
        preferredChallenge: 'WEB_AUTHN',
      },
    });
  },
};
const formFields = {
  signIn: {
    password: {
      isRequired: false,
    },
  },
};
</script>

<template>
  <main>
    <authenticator :services="services" :form-fields="formFields">
      <template #default="{ user, signOut }">
        user: {{ user.username }}
        <amplify-button variation="primary" @click="signOut"
          >サインアウト</amplify-button
        >
        <button @click="() => associateWebAuthnCredential()">
          パスキーを登録
        </button>
      </template>
    </authenticator>
  </main>
</template>
