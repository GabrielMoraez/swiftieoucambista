<script lang="ts">
  import { onMount } from 'svelte';
  import TaylorFace from '../assets/taylor-face.webp'
  import BarCode from '../assets/barcode.png'
  import html2canvas from 'html2canvas';

  export let result
  export let profile
  export let favoriteMusic

  const captureComponent = async () => {
    const componentRef = document.getElementById('swiftie-card');

    const canvas = await html2canvas(componentRef, {
      backgroundColor: null
    });

    const dataURL = canvas.toDataURL('image/png');

    const link = document.createElement('a');
    link.href = dataURL;
    link.download = 'component.png';
    link.click();
  };


</script>

<div class="backdrop">
  <div class="card-container">
    {#if !result}
      <div>
        "Oh não! A tropa do Russomano te tirou da fila e você foi preso(a) :/"
      </div>
      <button class="button">Enviar para um amigo te tirar da cadeia</button>
    {:else}
      <div class="card-title">
        "Siga em frente e não esqueça sua carteirinha de Swiftie"
      </div>
      <div id="swiftie-card" class="swiftie-card">
        <div class="card-header">
          Swiftie ID Card
        </div>
        <div class="info-container">
          <div class="upper-container">
            <div class="text-container">
              <span class="card-name">
                {profile.display_name}
              </span>
              <span class="favorite-label">Música preferida:</span>
              <span class="favorite">{favoriteMusic}</span>
              <span class="card-expiration">
                Carteirinha válida até o seu próximo tweet
              </span>
            </div>
            <div>
              <img class="taylor-face" src={TaylorFace} />
            </div>
          </div>
          <div class="bottom-container">
            <div>
              <img class="barcode" src={BarCode} />
            </div>
          </div>
        </div>
        <div class="card-footer">
          OFFICIAL
        </div>
      </div>
      <button class="button" on:click={captureComponent}>Baixe sua carteirinha</button>
    {/if}
  </div>
</div>

<style>
  .backdrop {
    background-color: rgba(0,0,0,0.5);
    width: 100vw;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .card-container {
    padding: 20px;
    background-color: #FFF;
    color: #000;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .card-title {
    font-size: 25px;
    font-weight: 700;
    margin-bottom: 20px;
  }
  .button {
    margin-top: 20px;
    font-size: 20px;
    transition: ease-in-out 0.2s;
  }
  .button:hover {
    cursor: pointer;
    transform: scale(1.05);
    transition: ease-in-out 0.2s;
  }
  .swiftie-card {
    background-color: #FFF;
    border-radius: 10px;
    position: relative;
    max-width: 500px;
  }
  .card-header {
    background-color: #35C1C7;
    color: #FFF;
    font-size: 35px;
    font-weight: 700;
    border-radius: 10px 10px 0px 0px;
    padding: 10px 0px;
  }
  .info-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
  }
  .upper-container {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin-bottom: 10px;
  }
  .text-container {
    display: flex;
    flex-direction: column;
    justify-content: start;
    text-align: left;
    padding-left: 20px;
    padding-right: 20px;
  }
  .card-name {
    font-size: 25px;
    font-weight: 700;
    margin-bottom: 10px;
  }
  .card-expiration {
    font-size: 14px;
    font-weight: 400;
    font-style: italic;
  }
  .taylor-face {
    object-fit: cover;
    width: 200px;
  }
  .bottom-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    justify-content: center;
  }
  .barcode {
    width: 200px;
    height: 50px;
    object-fit: cover;
  }
  .card-footer {
    background-color: #35C1C7;
    color: #FFF;
    font-size: 20px;
    font-weight: 700;
    border-radius: 0px 0px 10px 10px;
  }
  .favorite-label {
    font-size: 14px;
    font-weight: 400;
  }
  .favorite {
    font-size: 20px;
    font-weight: 700;
    margin-bottom: 10px;
  }

</style>