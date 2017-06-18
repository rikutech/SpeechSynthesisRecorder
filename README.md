# SpeechSynthesisRecorder
Utilize `navigator.mediaDevices.getUserMedia()` and `MediaRecorder` to get audio output from `window.speechSynthesis.speak()` call as `ArrayBuffer`, `AudioBuffer`, `Blob`, `MediaSource`, `ReadableStream`, or other object or data types, see https://lists.w3.org/Archives/Public/public-speech-api/2017Jun/0000.html.

Usage 

    async function ttsRecorder(text, utteranceOptions, recorderOptions, dataType = void 0 /* "blob" : Blob; "arrayBuffer" : ArrayBuffer; "audioBuffer" : AudioBuffer;  "mediaSource" : MediaSource, "readableStream" : ReadableStream */) {
      if (dataType === undefined) throw new TypeError("dataType is undefined");
      const ttsRecorder = await new SpeechSynthesisRecorder(text, utteranceOptions, recorderOptions, dataType);

      const tts = await ttsRecorder.start();
      // return `MediaStream`
      if (dataType === "mediaStream") return ttsRecorder.start();
      const data = await tts[dataType]();

      return {tts, data}
    }

     let ttsRecording = ttsRecorder("The revolution will not be televised", {
        voice: "english-us espeak",
        lang: "en-US",
        pitch: .75,
        rate: 1
      }, {
        mimeType: "audio/webm; codecs=opus"
      }, "mediaStream" /* "blob" : Blob; "arrayBuffer" : ArrayBuffer; "audioBuffer" : AudioBuffer;  "mediaSource" : MediaSource, "readableStream" : ReadableStream */);

     ttsRecording
     .then(({tts, data}) => {
      // do stuff with `ArrayBuffer`, `AudioBuffer`, `Blob`, `MediaSource`, `ReadableStream`
      console.log(tts, data);
      /*
      // `data` : `MediaSource`
      tts.audioNode.srcObj = data;
      tts.audioNode.title = tts.utterance.text;
      tts.audioNode.onloadedmetadata = () => {
        console.log(tts.audioNode.duration);
        tts.audioNode.play();
      }
      */
      /*
      // `data` : `ArrayBuffer`
      tts.audioNode.srcObj = data;
      tts.audioNode.title = tts.utterance.text;
      tts.audioNode.onloadedmetadata = () => {
        console.log(tts.audioNode.duration);
        tts.audioNode.play();
      }
      */
      /*
      // `data` : `AudioBuffer`
      let source = tts.audioContext.createBufferSource();
      source.buffer = ab;
      source.connect(tts.audioContext.destination);
      source.start()
      */
      /*
      // `data` : `Blob`
      tts.audioNode.src = URL.createObjectURL(blob);
      tts.audioNode.title = tts.utterance.text;
      tts.audioNode.onloadedmetadata = () => {
        console.log(tts.audioNode.duration);
        tts.audioNode.play();
      }
      */
      /*
      // `data` : `ReabableStream`
      data.getReader().read().then(d => { 
        // do stuff with stream
        tts.audioNode.src = URL.createObjectURL(d.value[0])
      })
      */
      /*
      // `data` : `MediaStream`
      // do stuff with active `MediaStream`
     })
     .catch(err => console.log(err))
