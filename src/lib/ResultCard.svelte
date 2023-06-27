<script lang="ts">
  let result
</script>

<div class="backdrop">
  <div class="card-container">
    {#if !result}
      <div>
        "Oh não! A tropa do Russomano te tirou da fila e você foi preso(a) :/"
      </div>
      <button class="button">Enviar para um amigo te tirar da cadeia</button>
    {:else}
      <div>
        "Siga em frente e não esqueça sua carteirinha de Swiftie"
      </div>
      <button class="button">Compartilhar sua carteirinha de Swiftie</button>
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
  }
  .button {
    margin-top: 20px;
  }
</style>