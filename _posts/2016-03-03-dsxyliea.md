---
layout: post
category : Programming
tags : [dyslexia, typoglycemia, Javascript]
title: Dsxiliea
---
{% include JB/setup %}

Una amiga que tiene dislexia me describió cómo ella experimenta leer. Ella *puede* leer, pero le cuesta mucho esfuerzo y concentración, y las letras parecen saltar de un lado a otro.

> Un  timador engañó a tres princesas para que enterraran tres cofres con lo que más querían. El timador las prometió que con los años los objetos de los cofres crecerían.

>Matilde, la princesa del palacio rosa, enterró un cofre lleno de  diamantes verdes con la esperanza de que estos crecieran.

>Lorena, la princesa del palacio azul, enterró su muñeca preferida esperando que esta creciera a la misma velocidad que ella.

>Concha, la princesa del palacio verde, enterró un cofre lleno de semillas.

>Cuando las princesas crecieron fueron a ver sus cofres. Matilde gritó al comprobar que el timador le había robado los diamantes. Lorena lloró al ver que su muñeca no había crecido y estaba estropeada por las lluvias. Concha por su parte se alegró al encontrarse un hermoso árbol que daba fruta con que alimentar a los ciudadanos de su reino

*Fuente: [MundoPrimaria](https://www.mundoprimaria.com/lecturas-para-ninos-primaria/princesa-las-semillas)*




<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
<script type="text/javascript">

"use strict";

$(function(){

	var getTextNodesIn = function(el) {
	    return $(el).find(":not(iframe,script)").addBack().contents().filter(function() {
	        return this.nodeType == 3;
	    });
	};

	// var textNodes = getTextNodesIn($("p, h1, h2, h3"));
	var textNodes = getTextNodesIn($("*"));



	function isLetter(char) {
		return /^[\d]$/.test(char);
	}


	var wordsInTextNodes = [];
	for (var i = 0; i < textNodes.length; i++) {
		var node = textNodes[i];

		var words = []

		var re = /\w+/g;
		var match;
		while ((match = re.exec(node.nodeValue)) != null) {

			var word = match[0];
			var position = match.index;

			words.push({
				length: word.length,
				position: position
			});
		}

		wordsInTextNodes[i] = words;
	};


	function messUpWords () {

		for (var i = 0; i < textNodes.length; i++) {

			var node = textNodes[i];

			for (var j = 0; j < wordsInTextNodes[i].length; j++) {

				// Only change a tenth of the words each round.
				if (Math.random() > 1/10) {

					continue;
				}

				var wordMeta = wordsInTextNodes[i][j];

				var word = node.nodeValue.slice(wordMeta.position, wordMeta.position + wordMeta.length);
				var before = node.nodeValue.slice(0, wordMeta.position);
				var after  = node.nodeValue.slice(wordMeta.position + wordMeta.length);

				node.nodeValue = before + messUpWord(word) + after;
			};
		};
	}

	function messUpWord (word) {

		if (word.length < 3) {

			return word;
		}

		return word[0] + messUpMessyPart(word.slice(1, -1)) + word[word.length - 1];
	}

	function messUpMessyPart (messyPart) {

		if (messyPart.length < 2) {

			return messyPart;
		}

		var a, b;
		while (!(a < b)) {

			a = getRandomInt(0, messyPart.length - 1);
			b = getRandomInt(0, messyPart.length - 1);
		}

		return messyPart.slice(0, a) + messyPart[b] + messyPart.slice(a+1, b) + messyPart[a] + messyPart.slice(b+1);
	}

	// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
	function getRandomInt(min, max) {
		
		return Math.floor(Math.random() * (max - min + 1) + min);
	}


	setInterval(messUpWords, 50);
});


</script>
