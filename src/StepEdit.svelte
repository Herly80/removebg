<script lang="ts">
    import "two-up-element"
    import { originalImage } from "./store";
</script>

<two-up>
<img src={$originalImage} alt="Imagen original subida por el usuario" />
<img src="" alt="Imagen sin fondo" />
</two-up>

<a download href="" class="bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold text-white rounded-full px-4 py-2">
    Descargar imagen sin fondo
</a>