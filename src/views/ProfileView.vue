<script setup lang="ts">
const route = useRoute();

const modalProfileFormOpen = ref(false);

const userAddress = computed(
  () => route.params.address.toLowerCase() as string
);

const { profiles, loadProfiles } = useProfiles();

onMounted(() => loadProfiles([userAddress.value]));
</script>

<template>
  <TheLayout>
    <template #sidebar-left>
      <ProfileSidebar
        :profiles="profiles"
        :user-address="userAddress"
        class="mb-4 lg:mb-0"
        @edit="modalProfileFormOpen = true"
      />
    </template>
    <template #content-right>
      <router-view
        :user-address="userAddress"
        :profile="profiles[userAddress]"
      />
    </template>
  </TheLayout>
  <teleport to="#modal">
    <ModalProfileForm
      :open="modalProfileFormOpen"
      :address="userAddress"
      :profile="profiles[userAddress]"
      @close="modalProfileFormOpen = false"
    />
  </teleport>
</template>
