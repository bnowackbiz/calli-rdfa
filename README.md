Calli-RDFa
==========

Interface
---------

```javascript
var context = {						// initial context for parsing fragments
	node: $("form#myForm")[0],		// optional, default is document node
	prefixes: {						// optional, will walk up to root once when/iff unknown prefixes are encountered
		rdf: "http://...", 
		foaf: "http://...", 
		...
	},
	base: "http://..."					// optional, default is page URL
};

// use callback

var triples = [];

var callback = function(subject, predicate, object, context) {
	var isBlankSubject = (subject.type == "bnode");
	var lang = object.language;
	var datatype = object.datatype;
	
	triples[triples.length] = { s: subject, p: predicate, o: object };
}

RDFa.parse(callback, context);

// let the lib build an index??

var graph = RDFa.parse(context);

// parse a snippet
var context = {
	node: $('<div xmlsn="...">.... </div>')
}
var graph = RDFa.parse(context);
```