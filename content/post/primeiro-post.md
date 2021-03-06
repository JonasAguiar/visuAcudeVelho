---
title: "Caminhada Feminina"
author: "Author Name"
tags: ["caminhada", "feminina"]
date: 2017-12-12T16:34:07-03:00
draft: true
---

Nada melhor mulheres do que caminhar com outras mulheres para conversar, bater aquele papo que só sai entre vocês, e para isso temos essa visualização, onde mostra qual hora do dia mais mulheres caminham, e você pode escolher um horário e fazer novas amizades.

<div class="{{ if .Get "class" }}{{ .Get "class" }}{{ else }}defaultclass{{ end }}">

<html lang="pt-br">

<head>
  <meta charset="utf-8">
  <title>Mulheres que caminham</title>
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
<script src="https://d3js.org/d3-random.v1.min.js"></script>

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
                  .domain([0, 420])  // de um domínio de 1 a 5
                  .rangeRound([0, height]);
    var color_scale = d3.scaleLinear()
                        .domain([0,420])
                        .range(["#cc0044", "#ffb3cc"]);


    function desenhaBarras(dados) {

      canvas.selectAll("circle")
        .data(dados)
        .enter()
              .append("circle")
              .attr("r", function(d){return escala_x(d.mulheres_pedestres)/5;})
              .attr("cx", function(d,i){ return i * 40 + 100;})
              .attr("cy", 300)
              .attr("fill", function(d){return color_scale(d.mulheres_pedestres)})

      canvas.selectAll("text")
            .data(dados)
            .enter()
                  .append("text")
                  .attr("width",30 )
                  .attr("x", function(d,i){return i *40  + 80   ;})
                  .attr("y", 500)
                  .attr("fill", "#cc0044")
                  .attr("font-size", "10px")
                  .attr("font-family", "comic-sans")
                  .text(function(d){return d.horario_inicial})


    }



    d3.tsv('mulherespedestres.csv', function(dados) {
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

</div>
