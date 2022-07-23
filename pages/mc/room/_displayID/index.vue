<template>
  <div class="w-full max-w-sm md:max-w-7xl m-auto mb-5">
    <LoadingOverlay v-if="loading" />
    <p class="mb-5">
      <NuxtLink to="/mc">ホーム</NuxtLink> / {{ state.roomName.value }}
    </p>
    <div class="flex flex-col gap-10">
      <!-- Details -->
      <div class="flex flex-col md:flex-row gap-10">
        <div class="md:w-3/5 self-stretch">
          <Container>
            <template #content>
              <div class="flex items-end gap-x-3">
                <HeaderText text="Detail" />
                <NuxtLink
                  :to="`/mc/room/${roomId}/settings`"
                  class="text-yellow"
                >
                  &gt;&gt; ルームを編集する
                </NuxtLink>
              </div>
              <ul class="mt-3 mb-3">
                <li>ルームネーム: {{ state.roomName.value }}</li>
                <li>ルーム説明: {{ state.description.value }}</li>
                <li>使用サービス: {{ state.provider.value || '未設定' }}</li>
                <li>
                  ルームURL:
                  <NuxtLink :to="`/room/${roomId}`" class="underline">{{
                    protocolAndHostname + '/room/' + roomId
                  }}</NuxtLink>
                </li>
              </ul>
            </template>
          </Container>
        </div>
        <!-- Request -->
        <Container>
          <template #content>
            <div class="flex items-end gap-x-3">
              <HeaderText text="Request" />
              <a
                :href="state.playlistId.value"
                target="_blank"
                class="text-yellow"
              >
                &gt;&gt; プレイリストを見る
              </a>
            </div>
            <!-- Table -->
            <table class="mt-3 w-full border-collapse">
              <thead class="hidden md:table-header-group text-left">
                <tr>
                  <th>Song</th>
                  <th></th>
                  <th>Artist</th>
                  <th>Album</th>
                  <th>Time</th>
                  <th>RadioName</th>
                  <th>Link</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="music in state.musics.value"
                  :key="music.providedMusicId"
                  class="border border-gray-400 hover:bg-neon-blue cursor-pointer w-full"
                  @click="redirectOutside(music.providerUrl)"
                >
                  <td class="w-14">
                    <img
                      :src="music.imageUrl"
                      class="w-14 h-14 mt-1 ml-1 mb-1 object-cover"
                    />
                  </td>
                  <td class="pl-3">
                    {{ music.musicTitle }}
                    <div class="text-sm md:hidden">
                      {{ music.artistName }}
                    </div>
                  </td>
                  <td class="hidden md:table-cell">{{ music.artistName }}</td>
                  <td class="hidden md:table-cell">{{ music.albumName }}</td>
                  <td class="hidden md:table-cell">{{ music.duration }}</td>
                  <td class="hidden md:table-cell w-1/6 text-sm">
                    {{ music.radioNames.join(',') }}
                  </td>
                  <td>
                    <a :href="music.providerUrl" target="_blank">
                      <img
                        src="~/assets/img/copy_url.svg"
                        class="w-4 ml-2 mr-2"
                      />
                    </a>
                  </td>
                </tr>
              </tbody>
            </table>
          </template>
        </Container>
      </div>
      <!-- Message -->
      <Container>
        <template #content>
          <HeaderText text="Letter" />
          <table class="mt-3 w-full border-collapse">
            <thead>
              <tr class="text-left">
                <th>Name</th>
                <th>Message</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="letter in state.letters.value"
                :key="letter.id"
                class="border border-gray-400 hover:bg-neon-blue"
              >
                <td class="pl-2 pt-2 pb-2">{{ letter.radioName }}</td>
                <td>{{ letter.message }}</td>
              </tr>
            </tbody>
          </table>
        </template>
      </Container>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  toRefs,
  reactive,
  computed,
  useRoute,
  onBeforeMount,
  watch,
} from '@nuxtjs/composition-api'
import { useLoading } from '~/core/03-composables/useLoading'
import { FetchRoomDetailRepository } from '~/core/02-repositories/fetchRoomDetail'
import { useFetchRoomDetail } from '~/core/03-composables/useFetchRoomDetail'

interface RequestMusicDetail {
  musicTitle: string
  albumName: string
  artistName: string
  imageUrl: string
  providerUrl: string
  duration: number
  providedMusicId: string
  radioNames: string[]
}

interface Letter {
  radioName: string
  message: string
}

interface State {
  roomName: string
  description: string
  provider: string | null
  playlistId: string | null
  musics: Array<RequestMusicDetail>
  letters: Array<Letter>
}

export default defineComponent({
  setup() {
    const route = useRoute()
    const protocolAndHostname = computed(
      () => window.location.protocol + '//' + window.location.hostname
    )
    const roomId = computed(() => route.value.params.displayID)
    const redirectOutside = (url: string) => {
      window.open(url, '_blank')
    }
    const { loading, setLoading } = useLoading()
    const { fetchRoomResponse, fetchRoomError, fetchRoom } = useFetchRoomDetail(
      new FetchRoomDetailRepository()
    )
    const state = toRefs(
      reactive<State>({
        roomName: '',
        description: '',
        provider: '',
        playlistId: '',
        musics: [
          {
            musicTitle: 'ヒステリックナイトガール',
            albumName: 'Styley!',
            artistName: 'PSUQUI',
            imageUrl:
              'https://i.scdn.co/image/ab67616d00001e021a8bba168d85553d9b2d47a9',
            providerUrl:
              'https://i.scdn.co/image/ab67616d00001e021a8bba168d85553d9b2d47a9',
            duration: 220,
            providedMusicId: 'm2ed2',
            radioNames: ['がっしー', 'てぃーてぃー'],
          },
          {
            musicTitle: 'ヒステリックナイトガール',
            artistName: 'PSUQUI',
            albumName: 'Styley!',
            imageUrl:
              'https://i.scdn.co/image/ab67616d00001e021a8bba168d85553d9b2d47a9',
            providerUrl:
              'https://i.scdn.co/image/ab67616d00001e021a8bba168d85553d9b2d47a9',
            duration: 220,
            providedMusicId: 'm2edsasa2',
            radioNames: ['がっしー'],
          },
        ],
        letters: [],
      })
    )
    onBeforeMount(() => {
      setLoading(true)
      fetchRoom({ roomId: roomId.value })
    })
    watch(fetchRoomResponse, () => {
      setLoading(false)
      state.roomName.value = fetchRoomResponse.value?.name as string
      state.description.value = fetchRoomResponse.value?.description as string
      state.provider.value = fetchRoomResponse.value?.provider as string
      state.letters.value = fetchRoomResponse.value?.letters as Letter[]
    })
    watch(fetchRoomError, () => {
      setLoading(false)
      alert(`an error occurred: ${JSON.stringify(fetchRoomError.value)}`)
    })
    return {
      state,
      roomId,
      redirectOutside,
      protocolAndHostname,
      loading,
      setLoading,
    }
  },
})
</script>
