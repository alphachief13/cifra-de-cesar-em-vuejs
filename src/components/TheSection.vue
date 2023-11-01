//tudo que está escrito em text-area tem que ser mostrado em p



<template>
    <main>
        <div class="up">
            <div class="up-left">
                <label for="menu-select">Selecionar Modo</label>
            <select name="menu-select" id="menu-select" v-model="selecionado">
                <option>criptografar</option>
                <option>descriptografar</option>
            </select>
            </div>
            <div class="up-mid">
                <p>Deslocamento À Direita</p>
            <div>
                <button @click="decrementar">-</button>
                <input 
                type="number" 
                min="0" 
                max="26" 
                placeholder="0-25"
                v-model="deslocamento"
                @input="criptografar"
            >
            <button @click="incrementar">+</button>
            </div>
            </div>
            <div class="up-right" id="pc">
                <p>Copiar</p>
                <button  @click="copiarTexto" id="btn-r">Copiar</button>
            </div>
        </div>


        <div class="down">
            <textarea 
                name="texto-para-codificar" 
                id="texto-para-codificar" 
                cols="30" 
                rows="10" 
                placeholder="Texto Para Codificar"
                v-model="texto"
                @input="criptografar"
                >
            </textarea>
        <div class="divTextoCodificado">
            <p id="texto-para-copiar">{{ textoCriptografado }}</p>
        </div>
        </div>

        <div class="mobile">
            <button @click="copiarTexto" id="btn-r">Copiar</button>
        </div>

        
        
    </main>
</template>


<script>

import { ref } from 'vue';

export default{

setup(){
    const deslocamento = ref(0)
    const texto = ref('')
    const textoCriptografado = ref('')
    const alfabetoArray = "abcdefghijklmnopqrstuvwxyz".split("")
    const selecionado = ref('criptografar')

    //transformo o texto em um array
    //se letra atual é igual a letra do alfabeto, a letra atual vai ser igual a letra que está no indice atual + deslocamento


    function criptografar(){
        if(selecionado.value == "criptografar"){
            const textoArray = texto.value.toLowerCase().split("")
            textoCriptografado.value = ""
            textoCriptografado.value = textoCriptografado.value.split("")

            textoArray.forEach((element)=> {

                if (!(element >= 'a' && element <= 'z')){
                    textoCriptografado.value.push(element)
                }

                for(let i = 0; i < alfabetoArray.length; i++){
                    if(element == alfabetoArray[i]){
                        if(i + deslocamento.value > 25){
                            textoCriptografado.value.push(alfabetoArray[(i+deslocamento.value)-26])
                        } else{
                            textoCriptografado.value.push(alfabetoArray[i + deslocamento.value])

                        }
                    }
                }

            })

            textoCriptografado.value = textoCriptografado.value.join("")

        } else{

            const textoArray = texto.value.toLowerCase().split("")
            textoCriptografado.value = ""
            textoCriptografado.value = textoCriptografado.value.split("")

            textoArray.forEach((element)=> {

                if (!(element >= 'a' && element <= 'z')){
                    textoCriptografado.value.push(element)
                }

                for(let i = 0; i < alfabetoArray.length; i++){
                    if(element == alfabetoArray[i]){
                        if(i - deslocamento.value < 0){
                            textoCriptografado.value.push(alfabetoArray[26+(i - deslocamento.value)])
                        } else{
                            textoCriptografado.value.push(alfabetoArray[i - deslocamento.value])

                        }
                    }
                }

            })

            textoCriptografado.value = textoCriptografado.value.join("")

            
            }
        
            if(deslocamento.value > 25 || deslocamento.value < 0){
                textoCriptografado.value = "Intervalo Máximo: 0-25"
            }
    }

    function incrementar(){
        if(deslocamento.value <= 24){
            deslocamento.value++
            criptografar()
        }
    }

    function decrementar(){
        if(deslocamento.value > 0){
            deslocamento.value--
            criptografar()
        }
    }

    function copiarTexto() {
      const inputElement = document.createElement('input');
      inputElement.value = textoCriptografado.value;
      document.body.appendChild(inputElement);

      inputElement.select();
      inputElement.setSelectionRange(0, 99999); 

      document.execCommand('copy');

      document.body.removeChild(inputElement);

      alert('Texto Copiado!');

}  

    return{
        deslocamento,
        texto,
        textoCriptografado,
        criptografar,
        incrementar,
        decrementar,
        selecionado,
        copiarTexto
    }
}
}
</script>

<style>

@media only screen and (max-width: 768px) {
  /* Estilos para telas menores, como celulares */
  main{
        margin-top: 50px;

    }
    #pc{
        display: none;
    }

    .mobile{
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 20px;
        margin-bottom: 20px;
    }

    .up{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        margin-bottom: 25px;
        gap: 10px;
    }

    .up p{
        font-size: 12px;
    }

    .up-right{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100px;
    }

    #btn-r{
        cursor: pointer;
        padding: 7px 12px;
        font-size: 16px;
        font-weight: 500;
        line-height: 10px;
        border: 1px solid #1b1f2326;
        border-radius: 4px;
        color: white;
        background-color: #2ea44f;
                
    }

    #btn-r:hover{
        opacity: 0.9;
    }

    .up-left{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100px;
    }

    #menu-select{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-size: 16px;
    }

    .up-mid{
        display: flex;
        flex-direction: column;
        text-align: center;
    }

    .up input{
        width: 100px;
        height: 50px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
    }

    .up div{
        display: flex;
        gap: 10px;

    }

    .up div button{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-weight: bolder;
    }

    .down{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        
    }


    .divTextoCodificado{
        width: 300px;
        height: 100px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        resize: none;
        margin-top: 50px;
    }
    #texto-para-codificar{
        margin-top: 50px;
        width: 300px;
        height: 100px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        resize: none;
    }
}

@media only screen and (min-width: 769px) and (max-width: 1024px) {
  /* Estilos para tablets */
  main{
        margin-top: 50px;

    }
    #pc{
        display: none;
    }

    .mobile{
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 20px;
        margin-bottom: 20px;
    }

    .up{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        margin-bottom: 25px;
        gap: 10px;
    }

    .up p{
        font-size: 12px;
    }

    .up-right{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100px;
    }

    #btn-r{
        cursor: pointer;
        padding: 7px 12px;
        font-size: 16px;
        font-weight: 500;
        line-height: 10px;
        border: 1px solid #1b1f2326;
        border-radius: 4px;
        color: white;
        background-color: #2ea44f;
                
    }

    #btn-r:hover{
        opacity: 0.9;
    }

    .up-left{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100px;
    }

    #menu-select{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-size: 16px;
    }

    .up-mid{
        display: flex;
        flex-direction: column;
        text-align: center;
    }

    .up input{
        width: 100px;
        height: 50px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
    }

    .up div{
        display: flex;
        gap: 10px;

    }

    .up div button{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-weight: bolder;
    }

    .down{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        
    }


    .divTextoCodificado{
        width: 300px;
        height: 100px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        resize: none;
        margin-top: 50px;
    }
    #texto-para-codificar{
        margin-top: 50px;
        width: 300px;
        height: 100px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        resize: none;
    }
}

@media only screen and (min-width: 1025px) {
  /* Estilos para PCs ou computadores desktop */

  main{
        margin-top: 100px;

    }
    .up{
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 50px;
        gap: 50px;
    }

    .up-right{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 300px;
    }

    #btn-r{
        cursor: pointer;
        padding: 12px 20px;
        font-size: 16px;
        font-weight: 500;
        line-height: 20px;
        border: 1px solid #1b1f2326;
        border-radius: 4px;
        color: white;
        background-color: #2ea44f;
                
    }

    #btn-r:hover{
        opacity: 0.9;
    }

    .up-left{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 300px;
    }

    #menu-select{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-size: 16px;
    }

    .up-mid{
        display: flex;
        flex-direction: column;
        text-align: center;
    }

    .up input{
        width: 200px;
        height: 50px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
    }

    .up div{
        display: flex;
        gap: 10px;

    }

    .up div button{
        padding: 12px 20px;
        background-color: #f8f8f8;
        border: 2px solid #ccc;
        cursor: pointer;
        font-weight: bolder;
    }

    .down{
        display: flex;
        gap: 2%;
    
    }


    .divTextoCodificado{
        width: 48%;
        height: 150px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        resize: none;
        margin-right: 1%;
    }
    #texto-para-codificar{
        width: 48%;
        height: 150px;
        padding: 12px 20px;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: #f8f8f8;
        font-size: 16px;
        margin-left: 1%;
        resize: none;
    }

    .mobile{
        display: none;
    }

}


    
</style>