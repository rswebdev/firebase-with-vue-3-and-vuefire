<script setup lang="ts">
import { computed, ref } from 'vue'
import { deleteDoc, doc } from '@firebase/firestore'
import { useFirestore } from 'vuefire'
import BaseButton from '@/components/base/BaseButton.vue'
import BaseIcon from '@/components/base/BaseIcon.vue'
import BaseImageCard from '@/components/base/BaseImageCard.vue'
import BaseRating from '@/components/base/BaseRating.vue'
import CafeImage from '@/components/CafeImage.vue'

const props = defineProps<{
  docId: string
  description?: string
  favorite?: boolean
  location?: string
  name?: string
  price?: number
  rating?: number | string
}>()

const description = ref(props.description || 'No review yet')
const docId = ref(props.docId || true)
const favorite = ref(props.favorite || false)
// const location = ref(props.location || 'United States')
const name = ref(props.name || '')
const price = ref(props.price || 1)
const rating = ref(props.rating || 0)

const priceSymbol = computed(() => {
  switch (price.value) {
    case 1:
      return '$ - Less than $10'
    case 2:
      return '$$ - Between $10 to $30'
    case 3:
      return '$$$ - Between $30 to $60'
    case 4:
      return '$$$$ - More than $60'
    default:
      return 'No price defined'
  }
})

const db = useFirestore()

async function deleteCafe() {
  await deleteDoc(doc(db, 'cafes', props.docId))
}
</script>

<template>
  <BaseImageCard>
    <template v-slot:image>
      <CafeImage />
    </template>
    <template v-slot:title>
      {{ name }}
    </template>
    <template v-slot:subtitle v-if="favorite">
      <BaseIcon
        color="error"
        icon="mdi-fire-circle"
        size="small"
        class="mr-1"
      />
      <span class="mr-1">Favorite</span>
    </template>
    <template v-slot:rating>
      <BaseRating
        :model-value="rating"
        color="amber"
        density="compact"
        half-increments
        readonly
        size="small"
      />
      <div class="text-grey ms-2">{{ rating }}</div>
    </template>
    <template v-slot:description>
      <p>{{ description }}</p>
    </template>
    <template v-slot:price>
      {{ priceSymbol }}
    </template>
    <template v-slot:actions>
      <BaseButton color="primary" :to="`/cafe/${docId}`">
        <BaseIcon icon="mdi-pencil" class="mr-1" /> Edit
      </BaseButton>
      <BaseButton @click="deleteCafe" color="error" text>
        <BaseIcon icon="mdi-trash-can-outline" class="mr-1" />
        Delete
      </BaseButton>
    </template>
  </BaseImageCard>
</template>

<style></style>
