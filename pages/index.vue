<template lang="html">
  <div class="page">
    <div id="viewer" ref="viewer"/>
    <div id="video" ref="video">
      <iframe
        id="youtube"
        width="1280"
        height="480"
        src="https://www.youtube.com/embed/X8VfuwSp2eI?rel=0&amp;controls=0&amp;showinfo=0&amp;"
        frameborder="0"
        allowfullscreen />
    </div>
  </div>
</template>

<script>
export default {
  mounted(){
    let viewer = new this.$marzipano.Viewer(this.$refs.viewer, {
      controls: {
        mouseViewMode: 'drag'
      }
    })

    let source = this.$marzipano.ImageUrlSource.fromString("bg3.jpg");
    let geometry = new this.$marzipano.EquirectGeometry([{ width: 4000 }]);
    let limiter = this.$marzipano.RectilinearView.limit.traditional(1024, 100*Math.PI/180);
    let view = new this.$marzipano.RectilinearView({ yaw: Math.PI }, limiter);

    let scene = viewer.createScene({
      source,
      geometry,
      view,
      pinFirstLevel: true
    })

    scene.switchTo()

    let container = scene.hotspotContainer();

    container.createHotspot(this.$refs.video, {
      yaw: 0.05,
      pitch: -0.102
    },{
      perspective: {
        radius: 1640,
        extraTransforms: "rotateX(5deg)"
      }
    });

  }
}
</script>

<style lang="css">
#viewer{
  height: 100vh;
  width: 100vw;
}
</style>
