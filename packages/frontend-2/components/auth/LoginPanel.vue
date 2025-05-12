<template>
  <div class="min-h-screen flex flex-col md:flex-row" v-if="!isLoggedIn">
    <!-- Login Form Side -->
    <div class="w-full md:w-1/2 flex justify-center items-center p-6 bg-gradient-to-br from-slate-50 to-slate-100">
      <div class="w-full max-w-md space-y-8">
        <div class="text-center mb-8">
          <h1 class="text-3xl font-bold text-slate-800 mb-2">{{ title }}</h1>
          <p class="text-slate-500">{{ subtitle }}</p>
        </div>

        <!-- Workspace Invite Header -->
        <AuthWorkspaceInviteHeader v-if="workspaceInvite" :invite="workspaceInvite" />

        <!-- Third Party Login Buttons -->
        <AuthThirdPartyLoginBlock
          v-if="hasThirdPartyStrategies && serverInfo"
          :server-info="serverInfo"
          :challenge="challenge"
          :app-id="appId"
          :newsletter-consent="false"
          custom-class="space-y-3"
        />

        <!-- SSO Button -->
        <FormButton
          v-if="isSsoEnabled"
          color="outline"
          full-width
          size="lg"
          :to="ssoLoginRoute"
          class="flex items-center justify-center gap-2 py-2.5"
        >
          <span>Continue with SSO</span>
        </FormButton>

        <!-- Divider -->
        <div class="relative my-6" v-if="hasThirdPartyStrategies || isSsoEnabled">
          <div class="absolute inset-0 flex items-center">
            <div class="w-full border-t border-slate-200"></div>
          </div>
          <div class="relative flex justify-center text-sm">
            <span class="px-4 bg-foundation text-slate-500">Or sign in with email</span>
          </div>
        </div>

        <!-- Email Login Form -->
        <AuthLoginWithEmailBlock
          v-if="hasLocalStrategy"
          :challenge="challenge"
          :workspace-invite="workspaceInvite || undefined"
          custom-style="space-y-6"
        />

        <!-- Sign up link -->
        <div
          v-if="!forcedInviteEmail"
          class="text-center text-sm text-slate-500 mt-6"
        >
          Don't have an account?
          <NuxtLink class="text-primary hover:text-primary-focus hover:underline font-medium" :to="finalRegisterRoute">
            Create one now
          </NuxtLink>
        </div>
      </div>
    </div>

    <!-- Showcase Side -->
    <div class="hidden md:flex md:w-1/2 bg-blue-600 text-white p-12 relative overflow-hidden">
      <!-- Blueprint Pattern Background -->
      <div class="absolute inset-0 opacity-5">
        <div class="absolute inset-0 pattern-blueprint"></div>
      </div>

      <!-- Content -->
      <div class="relative z-10 h-full flex flex-col w-full">
        <div class="flex items-center mb-12">
          <IconAccount class="w-9 h-9 mr-3" />
          <h1 class="text-2xl font-bold">Speckle</h1>
        </div>
        
        <div class="flex-grow flex flex-col justify-center">
          <h2 class="text-4xl font-bold mb-6 leading-tight">
            Build better together with Speckle
          </h2>
          <p class="text-primary-lightest text-lg mb-8 max-w-md">
            The open-source data platform for AEC. Access your 3D models and collaborate with your team in real time.
          </p>
          
          <div class="space-y-4 mb-12">
            <div v-for="(feature, index) in features" :key="index" class="flex items-center">
              <span class="w-6 h-6 rounded-full bg-primary-lighter flex items-center justify-center mr-3">
                <CheckIcon class="w-4 h-4" />
              </span>
              <span>{{ feature }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else />
</template>

<script setup lang="ts">
import { useQuery } from '@vue/apollo-composable'
import { AuthStrategy } from '~~/lib/auth/helpers/strategies'
import { useLoginOrRegisterUtils, useAuthManager } from '~~/lib/auth/composables/auth'
import { LayoutDialog } from '@speckle/ui-components'
import { registerRoute, ssoLoginRoute } from '~~/lib/common/helpers/route'
import {
  authLoginPanelQuery,
  authLoginPanelWorkspaceInviteQuery
} from '~/lib/auth/graphql/queries'
import { CheckIcon } from '@heroicons/vue/24/solid'
import IconAccount from '~/components/global/icon/Account.vue'

const props = withDefaults(
  defineProps<{
    dialogMode?: boolean
    title?: string
    subtitle?: string
  }>(),
  {
    dialogMode: false,
    title: 'Welcome back to Fusio',
    subtitle: 'Sign in to your account'
  }
)

const { appId, challenge } = useLoginOrRegisterUtils()
const { isLoggedIn } = useActiveUser()
const { inviteToken } = useAuthManager()
const router = useRouter()
const isWorkspacesEnabled = useIsWorkspacesEnabled()
const isSsoEnabled = useIsWorkspacesSsoEnabled()

const { result } = useQuery(authLoginPanelQuery)

const { result: workspaceInviteResult } = useQuery(
  authLoginPanelWorkspaceInviteQuery,
  () => ({
    token: inviteToken.value
  }),
  () => ({
    enabled: isWorkspacesEnabled.value
  })
)

const finalRegisterRoute = computed(() => {
  const result = router.resolve({
    path: registerRoute,
    query: inviteToken.value ? { token: inviteToken.value } : {}
  })
  return result.fullPath
})

const concreteComponent = computed(() => {
  return props.dialogMode ? LayoutDialog : 'div'
})

const workspaceInvite = computed(() => workspaceInviteResult.value?.workspaceInvite)
const forcedInviteEmail = computed(() => workspaceInvite.value?.email)

const serverInfo = computed(() => result.value?.serverInfo)
const hasLocalStrategy = computed(() =>
  (serverInfo.value?.authStrategies || []).some((s) => s.id === AuthStrategy.Local)
)

const hasThirdPartyStrategies = computed(() =>
  serverInfo.value?.authStrategies.some((s) => s.id !== AuthStrategy.Local)
)

// Features list for the showcase side
const features = [
  'Real-time collaboration',
  'Open-source platform',
  'Connects all your design tools',
  'Version control for AEC data'
]
</script>

<style scoped>
.pattern-blueprint {
  background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='1'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(0px);
  }
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}
</style>
