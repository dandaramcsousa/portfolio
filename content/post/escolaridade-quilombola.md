---
title: "Estabelecimentos de Ensino Fundamental em áreas remanescentes de Quilombos"
date: 2018-02-25
tags: [quilombo,palmares,mapas,visualizacao]
draft: false
---
<meta charset="utf-8">
  Há muitos anos atrás o Brasil enquanto um país com escravidão vivenciou a existência de <a href="https://www.estudopratico.com.br/o-que-e-um-quilombo/">quilombos</a>. Um dos mais famosos foi o Quilombo dos Palmares, que estava onde atualmente é Alagoas e abrigou milhares de negros refugiados. Os lugares onde eram quilombos são até hoje vividos com culturas daquela época, com forte raiz na cultura africana.

  A motivação para esse post veio do decreto que regulamenta terras quilombolas e que estava sendo ameaçado de ser derrubado. Muitos não sabem mas desde 2004 o PFL (hoje DEM) contestava o decreto assinado pelo então presidente Lula em 2003 alegando que o decreto distorce a constituição e, portanto, invade a esfera reservada à lei provocando um aumento de despesas. Só no começo de fevereiro de 2018 o STF manteve o decreto e a população pode suspirar aliviada após mais uma tentativa de apropriação de suas terras. A notícia pode ser melhor lida <a href="https://www1.folha.uol.com.br/poder/2018/02/stf-mantem-decreto-que-regulamenta-terras-de-quilombolas.shtml"> aqui </a>.

  É possivel ver <a href="http://atlas.fgv.br/marcos/trabalho-e-escravidao/mapas/pequeno-mapa-dos-quilombos">aqui</a> onde estão localizadas as terras quilombolas brasileiras e abaixo observar a quantidade de escolas de ensino fundamental nelas. Aqui vale ressaltar a importância de manter as terras com a população negra uma vez que ela tem um valor enorme para o seu povo. Espera-se também que o ensino não só nessa região mostre a história do povo negro que lutou pela sua própria liberdade no Brasil.
<style>

.cidades {
  fill: none;
  stroke: #fff;
  stroke-linejoin: round;
}

path:hover, path.highlighted {
  fill: tomato;
}

div.tooltip {
  position: absolute;
  background-color: white;
  border: 1px solid black;
  color: black;
  font-family:"avenir next", Arial, sans-serif;
  padding: 4px 8px;
  display: none;
}

</style>

<svg width="1000" height="600" ></svg>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/topojson.v2.min.js"></script>
<script src="../../data/legenda-d3-cor.js"></script>
<script>

var svg = d3.select("svg"),
    width = +svg.attr("width"),
    height = +svg.attr("height");


var path = d3.geoPath();

// a escala de cores
var color = d3.scaleThreshold().domain([1,4,8,12, 16, 20, 24, 28]).range(d3.schemePuOr[8]);

// função aux definida em legenda-d3-cor.js
desenhaLegenda(0, 60, color, "N de E.F em áreas remanescentes de Quilombos")

d3.queue()
    .defer(d3.json, "../../data/geo4-municipios-e-ensino-simplificado.json")
    .await(ready);

function ready(error, dados) {
  if (error) throw error;

  var cidades = dados.features;

  svg.append("g")
    .attr("class", "cidades")
    .selectAll("path")
    .data(cidades)
    .enter()
    .append("path")
      .attr("fill", d => {let valor = d.properties["2016"]; return valor === "NA" ? '#F7C9B1' : color(valor)})
      .attr("d", path)
      .attr("stroke-width", 0.1)
      .on("mouseover",showTooltip)
      .on("mousemove",moveTooltip)
      .on("mouseout",hideTooltip)

}

// ZOOM

//create zoom handler
var zoom_handler = d3.zoom()
    .on("zoom", zoom_actions);

//specify what to do when zoom event listener is triggered
function zoom_actions(){
 d3.selectAll("path").attr("transform", d3.event.transform);
}

//add zoom behaviour to the svg element
//same as svg.call(zoom_handler);
zoom_handler(svg);


// TOOLTIP

//Create a tooltip, hidden at the start
var tooltip = d3.select("body").append("div").attr("class","tooltip");
//Position of the tooltip relative to the cursor
var tooltipOffset = {x: 5, y: -25};

function showTooltip(d) {
  moveTooltip();

  tooltip.style("display","block")
      .text(d.properties.Localidade + ": " + d.properties["2016"]);
}

//Move the tooltip to track the mouse
function moveTooltip() {
  tooltip.style("top",(d3.event.pageY+tooltipOffset.y)+"px")
      .style("left",(d3.event.pageX+tooltipOffset.x)+"px");
}

//Create a tooltip, hidden at the start
function hideTooltip() {
  tooltip.style("display","none");
}

</script>

<p> Comparando a região onde teve quilombos e a o gráfico acima, é possível ver que há poucas escolas nas áreas quilombolas. Como exemplo, União dos Palmares só tem uma escola de ensino fundamental. De forma geral apenas o Nordeste e cidades próximas possuem algum número significativo de escolas. O destaque fica para Itapecuru Mirim, no Maranhão, com 37 escolas e que <a href="http://www.portaldomunim.com.br/comunidade-quilombola-em-itapecuru-mirim-e-reconhecida-pelo-incra/"> teve sua comunidade reconhecida em 2014</a>.

<p> Os dados aqui usados podem ser encontrados <a href="http://www.observatoriodopne.org.br/downloads"> aqui</a>.

<p> Ps.: Para esse post fica o agradecimento a Martha Michelly que ensinou a fazer o pior join de todos os tempos. Linda <3
