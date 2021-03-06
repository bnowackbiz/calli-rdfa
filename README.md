Calli-RDFa
==========

* RDFaParser.js: A callback-based RDFa Parser for HTML documents and nodes. A fork of [Green Turtle](https://code.google.com/p/green-turtle/) by R. Alexander Milowski
* RDFXMLSerializer: A simple RDF/XML Serializer aligned with the RDFa parser's callback.

Parser sample code
------------------

    var callback = function(subject, property, value, datatype, lang) {
		var isBlankSubject = subject.indexOf('_:') == 0;
		var isLiteral = datatype != null;
		var htmlElementCompletingThisTriple = this;
	}
	
	var node = document.documentElement; // any DOM node

	var parser = new RDFaParser();

	// manually set prefixes, if not defined in the ancestor markup of the node
	parser.setPrefix("ex", "http://example.com/schema");

	// start the parser with the two mandatory parameters "node" and "callback"
	if (!parser.parse(node, callback)) {
		throw 'could not parse';
	}

	// start on a sub-node
	parser.parse(document.getElementById('some-node'), callback);

	// parse a detached, in-memory node
	var el = document.createElement('div');
	el.innerHTML = '<a rel="ex:link" href="location.htm">a link</a>';
	parser.parse(el, callback);

Serializer sample code
----------------------

    var serializer = new RDFXMLSerializer();
	var parser = new RDFaParser();

	// on-the-fly serialization
    var callback = function(s, p, o, dt, lang) {
		// update the serializer's prefix mappings
		serializer.setMappings(parser.getMappings());

		// add incoming triples directly to the serializer
		serializer.addTriple(s, p, o, dt, lang);
	};

	parser.parse(document, callback);
	var rdfxml = serializer.toString();

	// 2-step approach
	var triples = [];
    var callback = function(s, p, o, dt, lang) {
		triples.push({s: s, p: p, o: o, dt: dt, lang: lang});
	};

	parser.parse(document, callback);
	serializer.setMappings(parser.getMappings());
	for (var i = 0, imax = triples.length, t; i < imax; i++) {
		t = triples[i];
		serializer.addTriple(t.s, t.p, t.o, t.dt, t.lang);
	}
	var rdfxml = serializer.toString();


Changes compared to Green Turtle's parser
-----------------------------------------
* The parser is started manually.
* In-memory indexes and the RDFa API were dropped, the parser works with an event-based addTriple-callback .
* Replaces node.baseURI with IE-compatible getNodeBase() method, so might work with IE9.


Known Issues / Todos
------------
* IE8 may not support node.namespaceURI
* IE8 may not support getAttributeNode/getAttributeNodeNS
