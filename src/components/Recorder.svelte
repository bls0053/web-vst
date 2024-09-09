<script lang="ts">
	import Icon from "@iconify/svelte";
	import { onMount } from "svelte";
	import EffectCol from "./EffectCol.svelte";
    import EffectLabel from "./EffectLabel.svelte";
	import EffectRow from "./EffectRow.svelte";
	import EffectCont from "./EffectCont.svelte";
	import IncrementEffect from "./IncrementEffect.svelte";
	import EffectKnob2 from "./EffectKnob2.svelte";



    export let masterNode: GainNode;
    export let audioContext: AudioContext;

    let timeoutId: number | null = null;
    let tempo = 160;
    let beatPer = 4;
    let bars = 4;
    let clickCount = 0;
    let clickInterval: number;
    let mediaRecorder: MediaRecorder | null = null;
    let audioChunks: BlobPart[] = [];
    let recordedAudioBuffer: AudioBuffer | null = null;
    let loopSource: AudioBufferSourceNode | null = null;
    let beat: number;
    let loopGain: number = 99;
    let clickSound: OscillatorNode;


    let gainNodeLoop: GainNode;

    $: if (tempo) {
        beat = (60 / tempo) * 1000;
    }
    
    function startMetronomeClick() {
        clickSound = audioContext.createOscillator();
        clickSound.frequency.value = 1000;

        const clickGain = audioContext.createGain();
        clickGain.gain.setValueAtTime(.5, audioContext.currentTime);
        clickGain.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.1);
    
        clickSound.connect(clickGain);
        clickGain.connect(audioContext.destination);
    
        clickSound.start();
        clickSound.stop(audioContext.currentTime + 0.1);
    }


    function changeMetronomeClick() {
        if (clickSound) {
            clickSound.type = "sawtooth"
        }
    };
    
    function playClicks() {
        clickCount = 0;
        console.log(clickCount)
        clickInterval = setInterval(() => {
            if (clickCount != 5) {
                startMetronomeClick();
                clickCount++;
            } 
            if (clickCount == 5) {
                clickCount++
                changeMetronomeClick();
                startMetronomeClick();
                startRecording();
            }
        }, beat);
    }
    
    function startRecording() {
        audioChunks = [];

        const dest = audioContext.createMediaStreamDestination();
        masterNode.connect(dest);
    
        mediaRecorder = new MediaRecorder(dest.stream);
        
        mediaRecorder.ondataavailable = (event) => {
            audioChunks.push(event.data);
        };
    
        mediaRecorder.onstop = async () => {
            const audioBlob = new Blob(audioChunks, { type: 'audio/webm' });
            const arrayBuffer = await audioBlob.arrayBuffer();
            recordedAudioBuffer = await audioContext.decodeAudioData(arrayBuffer);
            audioChunks = [];
        };
    
        mediaRecorder.start();
        console.log("Recording started");
    
        const recordingTime = (bars * beatPer * (60 / tempo)) * 1000;
        timeoutId = setTimeout(stopRecording, recordingTime);
    }
    
    function stopRecording() {
        clearInterval(clickInterval);
        if (timeoutId !== null) {
            clearTimeout(timeoutId);
            timeoutId = null;
        }
        if (!mediaRecorder) {
            return;
        }
        mediaRecorder.stop();
        console.log("Recording stopped");
    }
    
    function playLoop() {
        if (recordedAudioBuffer) {
            loopSource = audioContext.createBufferSource();
            loopSource.buffer = recordedAudioBuffer;
            loopSource.loop = true;

            gainNodeLoop = audioContext.createGain()
            loopSource.connect(gainNodeLoop)
            gainNodeLoop.connect(audioContext.destination)

            loopSource.start();
            console.log("Playback started");
        }
    }
    
    function stopLoop() {
        if (loopSource) {
            loopSource.stop();
            console.log("Loop stopped");
        }
    }

    $: if (gainNodeLoop) {
        gainNodeLoop.gain.value = loopGain / 100;
        console.log(gainNodeLoop.gain.value)
    }

 
    
</script>
    



<EffectCol size={6}>
    <EffectRow size={6}>
        <EffectCol size={4}>
        <EffectLabel>Record</EffectLabel>
            <EffectRow size={6}>
                <button on:click={playClicks}>
                    <Icon style="font-size: 30px ; color: red;" icon="ph:record-fill"/>
                </button>
                <button on:click={stopRecording}>
                    <Icon style="font-size: 30px ; color: red;" icon="ph:stop-fill"/>
                </button>
                <button on:click={playLoop}>
                    <Icon style="font-size: 30px ; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:play-fill"/>
                </button>
                <button on:click={stopLoop}>
                    <Icon style="font-size: 30px ; color: rgb(147 51 234 / var(--tw-bg-opacity));" icon="ph:pause-fill"/>
                </button>
            </EffectRow>
        </EffectCol>
        <EffectCol size={2}>
            <EffectLabel>Tempo</EffectLabel>
            <IncrementEffect bind:value={tempo} min={10} max={200} increment={5}></IncrementEffect>
        </EffectCol>
    </EffectRow>



        <EffectRow size={6}>
            <EffectCol>
                <EffectLabel>Beats</EffectLabel>
                <IncrementEffect bind:value={beatPer} min={1} max={12} increment={1}></IncrementEffect>
            </EffectCol>
            <EffectCol>
                <EffectLabel>Measures</EffectLabel>
                <IncrementEffect bind:value={bars} min={1} max={32} increment={1}></IncrementEffect>
            </EffectCol>
            <EffectCol>
                <EffectLabel>Gain</EffectLabel>
                <EffectRow>
                    <EffectKnob2 textSize={30} decimals={0} bind:value={loopGain} min={0} max={100}/>
                </EffectRow> 
            </EffectCol>
        </EffectRow>


</EffectCol>



<!-- tempo, gain,  -->