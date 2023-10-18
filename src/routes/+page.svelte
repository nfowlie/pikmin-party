<script>
    import '@picocss/pico'
    import pikmin from '$lib/assets/Pikmin-red.webp'
    import pikminSound from '$lib/assets/Pikmin sound effect-(p).flac'
  import { onMount } from 'svelte';

  let soundEffect
    onMount(()=>{
    soundEffect = new Audio(pikminSound)
})
    
const min = 1,
    max = 600
const setRandomTimeout = ()=>{
        let countdown = (Math.floor(Math.random() * (max-min))+ min)*1000
        let increment = 0
        let timer = setInterval(()=>{
            increment+= 1000
            console.log((countdown - increment)/1000)
        }, 1000)
        setTimeout(() => {
            clearInterval(timer)
            soundEffect.play()
            setRandomTimeout()
    }, countdown)
    }
</script>

<img src={pikmin} alt="Pikmin" height="374" width="344" on:click={()=>setRandomTimeout()}/>
