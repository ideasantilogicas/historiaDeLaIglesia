<template>
    <div>
        <div class="configuracionInicial">
            <div class="textoInicial">El concepto de <span class="variable">Dios</span> ya existe en el siglo 0 y la 
                <span class="variable">iglesia</span> aparece en una sociedad aún carente de 
                <span class="variable">culpa</span>. Se nos ha dicho que esta configuración seguirá 
                <span class="rojo">por los siglos de los siglos</span>.
                <br><br>La función <span class="funcion">historiaDeLaIglesia</span>() manipula los valores computacionalmente para mostrar el efecto de estos conceptos sobre la 
                <span class="variable">culpa</span> en la sociedad.
                <br><br>Al avanzar por los siglos, los valores crecen siguiendo el patrón Fibonacci,
                demostrando que <span class="rojo">la evolución de la <span class="variable">culpa</span> 
                se expande como espiral</span>, <span class="rojo">potencialmente hasta el infinito</span>.
                <br><br>
                <div class="inputContenedor">
                    <span style="color: rgb(255, 145, 2);">this</span>.siglos = 
                        <input type="number" class="input" v-model="siglos" id="input" min="0" max="33"
                               v-on:keyup="historiaDeLaIglesia" v-on:change="historiaDeLaIglesia">
                </div>
            </div>
            <img src="../assets/historiaIglesia.png" class="imagen">
        </div>
        <br>
        <div class="contenedorResultados">
            <div>
                <div class="resultado" v-for="resultado of resultados" :key="resultado.id">
                    <div> {{resultado.Dios}} </div>
                    <div> {{resultado.iglesia}} </div>
                    <div> {{resultado.culpa}} </div>
                </div>
            </div>
            <canvas id="lienzo"></canvas>
        </div>
    </div>
</template>

<script>
export default {
    name: 'HistoriaDeLaIglesia',
    data: function() {
        return {
            siglos      : 0,
            resultados  : [],
            resultadoID : 0,
            lienzo      : undefined,
            lienzoCtx   : undefined
        }
    },
    mounted: function() {
        this.lienzo                = document.getElementById("lienzo")
        this.lienzoCtx             = this.lienzo.getContext("2d")
        this.lienzoCtx.strokeStyle = "aqua"
        this.lienzoCtx.fillStyle   = "aqua"
        document.getElementById('input').focus()
    },
    methods: {
        historiaDeLaIglesia: function() {            
            this.resultados = []
            this.resultados.push( { Dios: 'Dios', iglesia: 'Iglesia', culpa: 'Culpa', id: this.resultadoID++ } )
            let Dios        = 1
            let iglesia     = 1
            let culpa       = 0

            for(let siglo = 0; siglo < this.siglos; siglo++) {
                Dios      = culpa + iglesia
                iglesia   = Dios  + culpa
                culpa     = Dios  + iglesia
                this.resultados.push( { Dios, iglesia, culpa, id: this.resultadoID++ } )
            }
            this.dibujarEspiral()
        },
        dibujarEspiral: function() {
            this.reiniciarLienzo()
            let x               = this.lienzo.width  / 2
            let y               = this.lienzo.height / 2
            let radio           = 1
            let radioFactor     = 0.5
            let anguloInicial   = 0
            let anguloFinal     = 0.5
            let direccion       = 0
            let direccionFactor = 0
            for(let i = 0; i < this.siglos; i++) {
                this.dibujarCurva(x, y, radio, anguloInicial, anguloFinal)
                radio         += radioFactor
                radioFactor   += (0.03 * i)
                anguloInicial += 0.5
                anguloFinal    = (anguloFinal   == 2)? 0 : anguloFinal   + 0.5
                if(anguloInicial == 2) {
                    anguloInicial = 0
                    anguloFinal   = 0.5
                }
                x = this.lienzo.width  / 2
                y = this.lienzo.height / 2
                if(direccion == 0) {
                    y -= direccionFactor
                    direccion++
                } else if(direccion == 1) {
                    x += direccionFactor
                    direccion++
                } else if(direccion == 2) {
                    y += direccionFactor
                    direccion++
                } else {
                    x -= direccionFactor
                    direccion = 0
                }
                direccionFactor += (i * 0.03)
            }
        },
        dibujarCurva: function(x, y, radio, anguloInicial, anguloFinal) {
            this.lienzoCtx.beginPath()
            this.lienzoCtx.arc(x, y, radio, anguloInicial * Math.PI, anguloFinal * Math.PI)
            this.lienzoCtx.stroke()
        },
        reiniciarLienzo: function() {
            this.lienzoCtx.clearRect(0, 0, this.lienzo.width, this.lienzo.height)
            var imagenFondo = new Image()
            imagenFondo.src = require("../assets/back.jpg")
            this.lienzoCtx.drawImage(imagenFondo,0,0, this.lienzo.width, this.lienzo.height)
            this.lienzoCtx.strokeText("Evolución de la culpa", 1, 15)
            this.lienzoCtx.font      = "10px Verdana"
            this.lienzoCtx.lineWidth = 1
        }
    }
}
</script>

<style scoped>

    .configuracionInicial {
        display: flex;
    }
    .textoInicial {
        margin-left: 10px;
        margin-right: 10px;
        text-align: left; 
        width: 63%;
        font-size: 25px;
    }
    .imagen {
        display: block;
        border-radius: 15px;
        border: solid 5px red;
        width: 30%;
    }

    .inputContenedor {
        display: flex;
        font-size: 20px;

    }
    .input {
        font-size: 20px;
        background-color: black;
        color: red;
        border-radius: 8px;
        margin-left: 10px;
        width: 50px;
    }

    .contenedorResultados {
        display: flex;
        justify-content: space-evenly;
        margin-right: auto;
        /* border: solid 1px lime; */
    }
    .resultado {
        display: flex;
        justify-content: space-evenly;
        font-size: 12px;
        border: solid 1px rgb(63, 62, 62);
        width: 480px;
    }

    .variable {
        color: rgb(11, 233, 233);
    }
    .rojo {
        color: rgb(200, 5, 5);
    }
    .funcion {
        color: rgb(124, 223, 43);
    }

    #lienzo {
        width: 1300px;
        height: 590px;
        /* border: solid 1px red; */
        border-radius: 15px;
    }

</style>