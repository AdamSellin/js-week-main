<template>
  <div class="Player max-w-full mx-6 md:mx-12 lg:mx-auto lg:max-w-5xl">
    <h1 class="text-5xl font-bold">Ma video</h1>

    <div class="Player__inner relative">
      <video @timeupdate="onUpdate" class="w-full" src="/assets/video.mp4" controls=true muted></video>

      <div class="Player__layer absolute top-0 left-0 w-full h-full text-left pointer-events-none p-12 text-white">
        <div class="text-4xl font-bold" v-if="currentLayer">
          <div v-if="!currentLayer.type">
            <h1>{{ currentLayer.content }}</h1>
          </div>
          
          <div v-if="currentLayer.type === 'img'">
            <img :src="currentLayer.content" alt="" class="absolute max-w-xs">
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import data from '@/assets/data.json'
export default {
  data() {
    return {
      currentLayer: null
    }
  },
  mounted() {
    console.log(data)
    // const video = this.$el.querySelector('video')
    // video.addEventListener('timeupdate', this.onUpdate)
  },
  methods: {
    onUpdate(e) {
      const video = e.target
      const currentTime = video.currentTime

      this.findCurrentLayer(currentTime)
    },

    findCurrentLayer(currentTime) {
      // il faut parser data.layers (for of)
      for (const layer of data.layers) {
        const start = layer.start
        const end = layer.start + layer.duration
        layer.end = end
        
        // On vérifie que le currentTime de la video est inclus entre la start et le end du layer, si oui on stock le layer dans la variable currentLayer
        if(currentTime >= start && currentTime <= end) {
          this.currentLayer = layer
        }
      }

      // si currentLayer existe, on va vérifier si le currentTime est plus petit que son start OU plus grand que son end. Si oui, on met currentLayer à null.
      if (this.currentLayer) {
        if (currentTime < this.currentLayer.start || currentTime > this.currentLayer.end) {
          this.currentLayer = null
        }
      }
      
      // Il faut regarder si current time il est supérier ou égale layer.start et inférieur à layer.end (start + duration)
    }


  }
}
</script>

<style lang="postcss" scoped>
/**
.Player {
  max-width: 100%;
  margin: 0px 24px;
}

.Player video {
  width: 100%;
}

@media (min-width: 768px) {
  .Player {
    margin: 0px 40px;
    opacity: 0.6;
  }
}

@media (min-width: 1024px) {
  .Player {
    margin: 0px auto;
    max-width: 900px;
    opacity: 1;
  }
}
 */
</style>