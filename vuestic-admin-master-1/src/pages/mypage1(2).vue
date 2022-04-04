<template>
  <div class="chat">
    <div class="row">
      <div class="flex xs12 md12">
        <va-card>
          <va-card-title>{{ $t('chat.title') }}</va-card-title>
          <va-card-content>
            <chat v-model="chatMessages"/>
          </va-card-content>
        </va-card>
      </div>
    </div>
  </div>
  <div class="cards">
    <div class="cards-container row d-flex wrap align--start">
      <template v-for="loop in listLoops" :key="loop + '-0'">
        <div class="flex xs12 sm6">
          <va-card>
            <va-card-title>{{ $t('cards.title.default') }}</va-card-title>
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6">
          <va-card>
            <va-card-title>
              {{ $t('cards.title.withControls') }}
              <va-spacer />
              <!-- <va-button class="mr-1" size="small" icon="refresh"/> -->
              <!-- <va-button size="small" icon="gear"/> -->
            </va-card-title>
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6">
          <va-card>
            <va-card-title>
              <!-- <va-icon class="mr-3" name="cogs" /> -->
              {{ $t('cards.title.customHeader') }}
            </va-card-title>
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6">
          <va-card>
            <va-card-content>
              <p>{{ $t('cards.title.withoutHeader') }}</p>
              {{ $t('cards.contentTextLong') }}
            </va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card>
            <va-image src="https://picsum.photos/300/200/?image=1043" style="height: 200px;"/>
            <va-card-title>{{ $t('cards.title.withImage') }}</va-card-title>
            <va-card-content>{{ $t('cards.contentText') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card>
            <va-image src="https://picsum.photos/300/200/?image=898" style="height: 200px;">
              <va-card-title text-color="#fff">{{ $t('cards.title.withTitleOnImage') }}</va-card-title>
            </va-image>
            <va-card-content>{{ $t('cards.contentText') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card>
            <va-image src="https://picsum.photos/300/200/?image=898" style="height: 200px;">
              <va-button class="ma-0">
                {{ $t('cards.button.readMore') }}
              </va-button>
            </va-image>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card
            stripe
            stripe-color="danger"
          >
            <va-card-title>{{ $t('cards.title.withStripe') }}</va-card-title>
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card
            color="success"
          >
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card
            color="danger"
          >
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card
            stripe
            stripe-color="info"
          >
            <va-card-title>{{ $t('cards.title.withStripe') }}</va-card-title>
            <va-card-content>{{ $t('cards.contentTextLong') }}</va-card-content>
          </va-card>
        </div>

        <div class="flex xs12 sm6 lg4 xl3">
          <va-card>
            <va-image src="https://picsum.photos/300/200/?image=1067" style="height: 200px;">
              <va-card-title text-color="#fff">{{ $t('cards.title.withTitleOnImage') }}</va-card-title>
            </va-image>
            <va-card-content>{{ $t('cards.contentText') }}</va-card-content>
          </va-card>
        </div>
      </template>
    </div>

    <va-inner-loading class="flex-center py-3" style="width: 100%;" :loading="isLoading">
      <va-button @click="addCards()">
        {{ $t('cards.button.showMore') }}
      </va-button>
    </va-inner-loading>
  </div>
</template>

<script>
import Chat from './admin/ui/chat/Chat.vue'

export default {
  name: 'chat-page',
  components: { Chat },
  data () {
    return {
      chatMessages: [
        {
          text: 'Hello! So glad you liked my work. Do you want me to shoot you?',
          yours: false,
        },
        {
          text: 'Yeah, that would be cool. Maybe this Sunday at 3 pm?',
          yours: true,
        },
        {
          text: 'Sounds great! See you later!',
          yours: false,
        },
        {
          text: 'Should I bring a lightbox with me?',
          yours: true,
        },
        {
          text: 'No, thanks. There is no need. Can we set up a meeting earlier?',
          yours: false,
        },
        {
          text: 'I\'m working on Vuestic, so let\'s meet at 3pm. Thanks!',
          yours: true,
        },

      ],
      listLoops: 1,
        counter: 1,
        isLoading: false,
    }
  },
  methods: {
    addCards () {
      this.isLoading = true
      setTimeout(() => {
        this.isLoading = false
        ++this.listLoops
      }, 1000)
    },
  },

}
</script>

<style lang='scss'>

</style>
