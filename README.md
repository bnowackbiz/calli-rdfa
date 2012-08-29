Calli-RDFa
==========

* RDFaParser.js: A callback-based RDFa Parser for HTML documents and nodes, based on [Green Turtle](https://code.google.com/p/green-turtle/) by R. Alexander Milowski
* RDFXMLSerializer: A simple RDF/XML Serializer based on the RDFa parser's callback.

Sample code
-----------

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



Changes compared to Green Turtle's parser
-----------------------------------------

* the parser is started manually
* in-memory indexes and the RDFa API were dropped, the parser works with an event-based addTriple-callback 
* replaces node.baseURI with IE-compatible getNodeBase() method


Known Issues / Todos
------------
* IE8 may not support node.namespaceURI
* IE8 may not support getAttributeNode/getAttributeNodeNS
