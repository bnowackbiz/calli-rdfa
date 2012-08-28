Calli-RDFa
==========

Interface
---------

    var callback = function(subject, property, value, datatype, lang) {
		var isBlankSubject = subject.indexOf('_:') == 0;
		var isLiteral = datatype != null;
		var htmlElementCompletingThisTriple = this;
	}
	
	var node = document.documentElement;

	var parser = new RDFaParser();
	parser.setPrefix("rdf", "http://www.w3.org/1999/02/22-rdf-syntax-ns#");

	var base = null;// use node or document base

	if (!parser.parse(node, callback, base)) {
		throw 'could not parse';
	}

