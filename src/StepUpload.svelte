<script lang="ts">
    import { ImageStatus } from './types.d';
    import { imageStatus, originalImage } from './store';
    import Dropzone from 'dropzone';
    import 'dropzone/dist/dropzone.css';

    import { onMount } from 'svelte';
    import { claim_svg_element } from 'svelte/internal';

    onMount(() => {
        const dropzone = new Dropzone('#dropzone', {
            uploadMultiple: false,
            acceptedFiles: '.jpg, .png, .webp',
            maxFiles: 1
        })

        dropzone.on('sending', (file, xhr, formData) => {
            imageStatus.set(ImageStatus.UPLOADING)
            //aqui podemos añadir la apiKey, configuracion
            formData.append('upload_preset', 'h3rly80')
            formData.append('timestamp', (Date.now() / 1000))
            formData.append('api_key', 547289569715651)
        })

        dropzone.on('success', (file, response) => {
            const {
                secure_url: url
            } = response
            
            imageStatus.set(ImageStatus.DONE)
            originalImage.set(url)

            //crear la imagen con fondo transparente 
            //guardar en el backgroundImage
            
            console.log(response)
        })

        dropzone.on('error', (file, response) => {
            console.log('HA IDO MAL')
            console.log(response)
        })
    }) 
  </script>

  <form id="dropzone" class="shadow-2xl border dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col" action="https://api.cloudinary.com/v1_1/dub6qyl4j/image/upload">
    
    {#if $imageStatus === ImageStatus.READY}
        
  
    <button class="font-bold pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4">
        Upload file
    </button>
    <strong class="text-lg mt-4 text-gray-800">or drop a file</strong>
    {/if}
    
    {#if $imageStatus === ImageStatus.UPLOADING}
    
    <strong class="text-lg mt-4 text-gray-800">Uploading files...</strong>
    {/if}

  </form>