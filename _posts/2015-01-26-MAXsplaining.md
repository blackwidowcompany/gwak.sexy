---
layout: post
title: MAXsplaining
---

Lot's of talk on Reddit regarding the number of MAXes pulled by certain outfits; thought I'd actually take a look at _#demnumbers_.

<style>
	.axis path,
	.axis line {
		fill: none;
		stroke: #ccc;
		stroke-dasharray: 1px 3px;
		shape-rendering: crispEdges;
		font-size: 8px;
	}

	.point {
		fill: steelblue;
		stroke: #000;
		stroke-width: 0;

	}

	.tooltip {
		position: relative;
		pointer-events: none;
	}

	svg {
		background: #f8f8f8;
	}
</style>

<div class='vis'></div>
<script>
	var width = 720;
	var height = 400;
	var padding = 50;

	var x = d3.scale.linear().range([padding, width - padding]);
	var y = d3.scale.linear().range([padding, height - padding]);

	var xAxis = d3.svg.axis()
		.scale(x)
		.orient('bottom');

	var yAxis = d3.svg.axis()
		.scale(y)
		.orient('left');

	var svg = d3.select('.vis').append('svg')
		.attr('width', width)
		.attr('height', height);

	var tooltip = d3.select('.vis').append('div')
		.attr('class', 'tooltip')
		.style('opacity', 0);

	d3.csv('/data/max-frequency.csv', function(data) {

		data.forEach(function(d) {
			d.max = +d.max;
			d.kdr = +d.kdr;
		});

		x.domain([0, d3.max(data, function(d) { return d.max; })]);
		y.domain([d3.max(data, function(d) { return d.kdr; }), 0]);

		svg.append('g')
			.attr('class', 'axis')
			.attr("transform", "translate(0," + (height - padding) + ")")
			.call(xAxis);

		svg.append('g')
			.attr('class', 'axis')
			.attr("transform", "translate(" + padding + ",0)")
			.call(yAxis);

		svg.selectAll('.point')
			.data(data)
			.enter()
			.append('circle')
			.attr('class', 'point')
			.attr('r', 5)
			.attr('cx', function(d) { return x(d.max); })
			.attr('cy', function(d) { return y(d.kdr); })
			.style('fill', function(d) {
				if (d.faction == 'tr') {
					return '#FF5A5A';
				} else if (d.faction == 'vs') {
					return '#C934FF';
				} else {
					return '#5A80FF';
				}
			})
			.on('mouseover', function(d) {
				tooltip.transition()
					.duration(200)
					.style('opacity', 0.9);
				tooltip.html("<p><span class='semibold'>" + d.tag + " —</span> KDR: " + d.kdr + " — MAX-percent: " + d.max + "</p>")
					.on('mouseout', function(d) {
						tooltip.transition()
						.duration(500)
						.style('opacity', 0);
				});
		});
	});
</script>

## The data

- I took the top 20 outfits for each faction in terms of active members from [stats.dasanfall](http://stats.dasanfall.com/ps2/outfits/?sort=active_members&server=17&min=12&max=9999)
- Average outfit KDR's come from the same place.
- The MAX data comes from [Kamerad's site](http://kamerad.servebeer.com/max-facts?q=%5Bangc%5D)

## Takeaways

- There is no TR alternative to GOKU (FRZA is their altfit) or BAX. We have some good outfits, but in terms of scale and in number of BR100's, no one is close. Not going to speculate on why that's the case, but it is.
- WILL is doing some weird shit with their MAXes.
- BWC is doing pretty well. I'm always worrying that we're not pulling our weight, but at least in terms of K/D, we're doing ok.
- AOD isn't represented. They have [8000 members](https://www.reddit.com/r/EmeraldPS2/comments/2tqtcy/aod_officially_reached_8000_members_last_night/). They don't render on [stats.dasanfall](http://stats.dasanfall.com/ps2/outfit/AOD). I don't even.

While the frequency at which MAX's are pulled seems to be positively correlated to average outfit KDR, it is in no way the only reason. Pulling MAX's more often __might__ raise your K/D, but, don't. __Please.__
