<template>
  <section>
    <canvas id='renderCanvas'></canvas>

    <div class="start">start</div>
  </section>
</template>

<script>
import * as BABYLON from 'babylonjs/es6'

export default {
  mounted() {
    var BjsApp = BjsApp || {}

    BjsApp.init = function() {
      //get the canvas
      var canvas = document.getElementById('renderCanvas')

      //create a BabylonJS engine object, true for antialias
      var engine = new BABYLON.Engine(canvas, true)

      //create a scene
      var scene = new BABYLON.Scene(engine)

      //create a camera
      var camera = new BABYLON.ArcRotateCamera(
        'camera',
        Math.PI,
        Math.PI/2 - 0.5,
        20,
        BABYLON.Vector3.Zero(),
        scene
      )

      //let the user move the camera
      camera.attachControl(canvas)
      camera.upperRadiusLimit = 50
      camera.anim = {
        speed: 0.01,
        angle: 0
      }
      camera.useAutoRotationBehavior = true;
      console.log(camera)

      //light
      var light = new BABYLON.HemisphericLight(
        'light1',
        new BABYLON.Vector3(0, 1, 0),
        scene
      )
      light.intensity = 0.5
      light.groundColor = new BABYLON.Color3(0, 0, 1)

      scene.clearColor = new BABYLON.Color3(0, 0, 0)

      //planets
      var dropMaterial1 = new BABYLON.StandardMaterial('planetMat', scene)
      dropMaterial1.diffuseTexture = new BABYLON.Texture(
        'images/texture/hmm.jpg',
        scene
      )
      dropMaterial1.specularColor = new BABYLON.Color3(0, 0, 0)

      //planets
      var dropMaterial2 = new BABYLON.StandardMaterial('planetMat', scene)
      dropMaterial2.diffuseTexture = new BABYLON.Texture(
        'images/texture/ahhh.jpg',
        scene
      )
      dropMaterial2.specularColor = new BABYLON.Color3(0, 0, 0)

      //planets
      var dropMaterial3 = new BABYLON.StandardMaterial('planetMat', scene)
      dropMaterial3.diffuseTexture = new BABYLON.Texture(
        'images/texture/brrr.jpg',
        scene
      )
      dropMaterial3.specularColor = new BABYLON.Color3(0, 0, 0)
      
      //planets
      var dropMaterial4 = new BABYLON.StandardMaterial('planetMat', scene)
      dropMaterial4.diffuseTexture = new BABYLON.Texture(
        'images/texture/grrr.jpg',
        scene
      )
      dropMaterial4.specularColor = new BABYLON.Color3(0, 0, 0)

      let cirle1 = BABYLON.Mesh.CreateSphere('cirle1', 16, 2, scene)
      cirle1.position.x = 4
      cirle1.material = dropMaterial1

      let circle2 = BABYLON.Mesh.CreateSphere('circle2', 16, 2, scene)
      circle2.position.z = 4
      circle2.material = dropMaterial2

      let circle3 = BABYLON.Mesh.CreateSphere('circle3', 16, 2, scene)
      circle3.position.x = -4
      circle3.material = dropMaterial3

      let circle4 = BABYLON.Mesh.CreateSphere('circle4', 16, 2, scene)
      circle4.position.z = -4
      circle4.material = dropMaterial4

      // skybox
      var skybox = BABYLON.Mesh.CreateBox('skybox', 1000, scene)
      var skyboxMaterial = new BABYLON.StandardMaterial('skyboxMat', scene)

      //dont render what we cant see
      skyboxMaterial.backFaceCulling = false

      //move with camera
      skybox.infiniteDistance = true

      skybox.material = skyboxMaterial

      //remove reflection in skybox
      skyboxMaterial.diffuseColor = new BABYLON.Color3(0, 0, 0)
      skyboxMaterial.specularColor = new BABYLON.Color3(0, 0, 0)

      //texture of 6 sides of the cube
      skyboxMaterial.reflectionTexture = new BABYLON.CubeTexture(
        '/images/texture/skybox',
        scene
      )
      skyboxMaterial.reflectionTexture.coordinatesMode =
        BABYLON.Texture.SKYBOX_MODE

      //thing method allows you to animate / move things
      scene.beforeRender = function() {
        // camera.alpha = camera.anim.angle
        // camera.anim.angle += camera.anim.speed
      }

      let ease = new BABYLON.CubicEase()
      ease.setEasingMode(BABYLON.EasingFunction.EASINGMODE_EASEINOUT)

      function animateCameraTo(targetX, targetY, targetZ, locationX, locationY, locationZ, speed, frameCount) {
        let cameraTarget = new BABYLON.Vector3(targetX , targetY, targetZ);
        let cameraPosition = new BABYLON.Vector3(locationX, locationY, locationZ)
        BABYLON.Animation.CreateAndStartAnimation('at4', camera, 'position', speed, frameCount, camera.position, cameraPosition, 0, ease);
        // BABYLON.Animation.CreateAndStartAnimation('at5', camera, 'target', speed, frameCount, camera.target, cameraTarget, 0, ease);
        console.log(camera.target)
      };

      engine.runRenderLoop(function() {
        scene.render()
        camera.upperRadiusLimit = 500
      })

      // the canvas/window resize event handler
      window.addEventListener('resize', function() {
        engine.resize()
      })

      document.querySelector('.start').addEventListener('click', () => {
        // animateCameraTo(0, 0, -45, 0, 0, 4, 0.1, 0.1)
        camera.alpha = Math.PI*2
      })
    }

    BjsApp.init()
  }
}
</script>
