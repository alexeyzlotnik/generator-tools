<script lang="ts" setup>
import type { HeaderLink } from '#ui-pro/types'

const { $csrfFetch } = useNuxtApp()

const { loggedIn, session, user } = useUserSession()

const title = useAppConfig().app.name;
const icon = useAppConfig().app.logo
</script>

<template>
  <UHeader
    :title
    :ui="{ logo: 'items-center' }"
  >
    <template #logo>
      <img
        class="h-6 w-6"
        :src="icon"
        aria-hidden
      >

      <span> {{ title }} </span>
    </template>

    <template #right>
      <!-- <UColorModeToggle /> -->
    </template>

    <!-- <template #right>
      <template v-if="loggedIn && user">
        <UDropdown
          :items="items"
          :popper="{ placement: 'bottom-end' }"
        >
          <UButton
            color="gray"
            aria-label="Profile picture of connected user"
            variant="ghost"
            square
          >
            <AppAvatar :src="user.avatar" />
          </UButton>
        </UDropdown>
      </template>
      <template v-else>
        <UButton
          to="/login"
          color="gray"
          variant="ghost"
        >
          Login
        </UButton>
        <UButton
          to="/register"
          color="black"
          variant="solid"
        >
          Register
        </UButton>
      </template>
    </template> -->
  </UHeader>
</template>
