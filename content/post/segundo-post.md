---
title: "Mulheres Ciclistas"
author: "Author Name"
tags: ["ciclismo", "mulheres"]
date: 2017-12-12T16:58:46-03:00
draft: true
---

Pegue sua bike, escolha o melhor horário e venha se juntar ao grupo de mulheres que andam de bicicleta ao redor do Açude.


<html lang="pt-br">

<head>
  <meta charset="utf-8">
  <title>Mulheres Ciclistas</title>
  <script src="https://d3js.org/d3.v4.min.js"></script>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
</head>

<body>
  <div class="container">
    <div class="row">
      <h2></h2>
    </div>
    <div class="row mychart" id="chart">
    </div>
  </div>

  <style>
    .mychart rect {
      fill: steelblue;
    }

    .mychart rect:hover {
      fill: goldenrod;
    }

    .mychart text {
      font: 30px sans-serif;
      text-anchor: left;
    }
  </style>

<script src="https://d3js.org/d3-time.v1.min.js"></script>
<script src="https://d3js.org/d3-time-format.v2.min.js"></script>

  <script type="text/javascript">
    "use strict"

    var formatoDahora = d3.timeFormat("%H:%M");
    var margin = {top: 80, right: 20, bottom: 80, left: 50}
    var width = 1400;
    var height = 500;
    var canvas = d3.select("body").append("svg")
                    .attr("width", width)
                    .attr("height", height)

    var escala_x = d3.scaleLinear()  // crie uma função de escala linear
                  .domain([0, 50])  // de um domínio de 1 a 5
                  .rangeRound([0, height]);
    var color_scale = d3.scaleLinear()
                        .domain([0,30])
                        .range(["#cc0044", "#ffb3cc"]);


    function desenhaBarras(dados) {

      canvas.selectAll("rect")
        .data(dados)
        .enter()
              .append("rect")
              .attr("width", 30 )
              .attr("height", function(d){return escala_x(d.mulheres_ciclistas);})
              .attr("x", function(d, i){return i *40  + 60   ;})
              .attr("y", function(d){return  height -escala_x(d.mulheres_ciclistas) -10 ;})
              .attr("fill", function(d){return color_scale(d.mulheres_ciclistas)})

      canvas.selectAll("text")
            .data(dados)
            .enter()
                  .append("text")
                  .attr("width",30 )
                  .attr("x", function(d,i){return i *40  + 65   ;})
                  .attr("y", 500)
                  .attr("fill", "black")
                  .attr("font-size", "10px")
                  .attr("font-family", "comic-sans")
                  .text(function(d){return d.horario_inicial})

     canvas.selectAll("numbers")
          .data(dados)
          .enter()
                  .append("text")
                  .attr("width",100 )
                  .attr("x", function(d,i){return i *40  + 70   ;})
                  .attr("y", function(d){return height -escala_x(d.mulheres_ciclistas) -15;})
                  .attr("font-family", "comic-sans")
                  .attr("fill", "#cc0044")
                  .text(function(d){return d.mulheres_ciclistas})



    }



    d3.tsv('mulheresbike.csv', function(dados) {
      dados['horario_inicial'] = formatoDahora(dados['horario_inicial']);
      desenhaBarras(dados);
    });


    /*
    * Assim como a função d3.tsv, existe a d3.csv e a d3.json
    * Em todas, o primeiro parâmetro é uma URL e o segundo a função que
    * será executada quando o dado for obtido.
    * >>>>> A chamada ao d3.tsv é assíncrona <<<<<<
    */
  </script>
</body>

</html>
