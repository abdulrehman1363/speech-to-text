<template>
  <!-- Voice Record Title + Button -->
  <div class="form-group row">
    <div class="col-md-12 align-items-center">
      <input type="file" @change="onFileChange">
      <button @click="recordAudio()" type="button" id="button_record" class="btn btn-danger">
        Record
      </button>
      <button type="button" id="button_stop" class="btn btn-success" @click="stop">
        Stop
      </button>
      <div id="audio" class="audio">
      </div>
    </div>
  </div>
  <div class="row" style="margin-top: 10px">
    <div class="col-md-12 align-items-center">
      <button type="button" class="btn btn-outline-success" style="width: 436px;">
        Upload
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recorder: null,
      items : [],
      files: null
    }
  },
  methods: {

    recordAudio() {
      let device = navigator.mediaDevices.getUserMedia({ audio: true });
      device.then((stream) => {

        this.recorder = new MediaRecorder(stream);
        this.recorder.start();
        this.recorder.ondataavailable = (e) => {
          this.items.push(e.data);
          if (this.recorder.state == "inactive") {
            let blob = new Blob(this.items, { type: "audio/*" });
            let audio = document.getElementById("audio");
            let mainaudio = document.createElement("audio");
            mainaudio.setAttribute("controls", "controls");
            audio.appendChild(mainaudio);
            mainaudio.src = URL.createObjectURL(blob);
          }
        };
      });
    },
    // called on button click
    stop() {
      this.recorder.stop()
    },
    onFileChange(e){
       this.files = e.target.files || e.dataTransfer.files;
      if (!this.files.length) return;
      console.log('file = ', this.files[0])
    }
  },
};
</script>

