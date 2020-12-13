<template>
  <div class='video_player'>
    <div class='video_player_area' @dblclick="dbclick" ref='videoBox'>
      <video 
        ref="videoPlayer"
        controls
        playsinline
        :src='src' 
        :width="width" 
        :height="height"
        @dblclick="dbclick"
      >
        Video player not available
      </video>
    </div>
    <div class='video_player_ctrl'>
      <div>
        <input type="file" id='videoPlayerInput' class='video_player_input' @change="getVideoSource" />
        <label for='videoPlayerInput'>{{ tipInfo || '选择视频文件' }}</label>
      </div>
      <div>
        <ul>
          <li>暂停</li>
          <li>播放</li>
          <li>快进</li>
          <li>快退</li>
          <li>播放速度</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'videoPlayer',
  data(){
    return {
      tipInfo:'',
      src:'aa', //视频src
      width:0, // video播放宽度
      ratio:[16,9], //长宽比
    }
  },
  computed:{
    height(){
      const ratio = this.ratio,
        widthRatio = ratio[0],
        heightRatio = ratio[1];
      return this.width * (heightRatio / widthRatio || 9/16)
    }
  },
  methods:{
    // 设置播放器的宽度
    setPlayerWidth(){
      const parentRect = this.$refs.videoPlayer.parentElement.getBoundingClientRect(),
        width = parentRect.width;
      this.width = width;
    },
    // 初始化并动态调整播放器宽度
    initPlayerWidth(){
      this.setPlayerWidth()
      const setPlayerWidth = this.$_tool.debounce(this.setPlayerWidth);
      window.addEventListener('resize',setPlayerWidth,false);
      // 销毁时移除监听事件
      this.$once('hook:beforeDestroy',function(){
        window.removeEventListener('resize',setPlayerWidth)
      })
    },
    // 获取视频资源
    getVideoSource(e){
      const file = e.target.files[0];
      if(file){
        let {name,size,type} = file;
        this.tipInfo = `name:${name}\nsize:${size}\ntype:${type}`;
        this.src = window.URL.createObjectURL(file)
      }
    },
    // 双击事件
    dbclick(){
    },
    // 
    test(){
      debugger
      const box = this.$refs.videoBox;
      box.addEventListener('click',e => {
        console.log('1')
        e.stopImmediatePropagation()
        e.stopPropagation()
      },false)
      box.addEventListener('click',() => {
        console.log('2')
      },false)
    }
  },
  mounted(){
    this.initPlayerWidth()
    this.test()
  }
}
</script>

<style scoped>
.video_player{
  display: flex;
}
.video_player_area{
  flex: auto;
}
.video_player_ctrl{
  flex: none;
}
.video_player_input{
  visibility: hidden;
}
/* 进度条 */
video::-webkit-media-controls-timeline {
    display: none;
}
/* 剩余时间 */
video::-webkit-media-controls-time-remaining-display {
    display: none;            
}    
/* 音量按钮 */
video::-webkit-media-controls-mute-button {
    display: none;            
}
video::-webkit-media-controls-toggle-closed-captions-button {
    display: none;            
}
/* 音量的控制条 */
video::-webkit-media-controls-volume-slider {
    display: none;            
}
</style>