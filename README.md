#pViz doc & examples
##What is pViz?
pViz is a <strike>protein</strike> sequence features viewer for modern browsers, based on a JavaScript library, SVG and css.

Given sequence and a list of positioned features, it displays the sequence and the features aligning them on the sequence.
Features are layed out not to overlaying each other, allowing zooming, interaction and a decent amount of customization.

Default sources can be explictely defined in JavaScript (limit is the sky),  [DAS servers](http://en.wikipedia.org/wiki/Distributed_Annotation_System), [PEFF](http://www.psidev.info/node/363) (PSI extended Fasta format) or a mix of them.

**Warning!** pViz target modern browsers, such as Firefox, Chrome and Safari. Do not expect it to work on IE7, just upgrade.

##Hello World

    <head>
      <link rel="stylesheet" type="text/css" href="../dist/pviz-core.css">
      <script src="../dist/pviz-bundle-min.js"></script>
    </head>
    <body>
      <div id="main"></div>
      <script>
        var pviz = this.pviz;
        var seqEntry = new pviz.SeqEntry({sequence : 'ABCDEFGIJKLMNOPQRSTUVWXYZ'});
        
        new pviz.SeqEntryAnnotInteractiveView({
          model : seqEntry,
          el : '#main'
          }).render();
          
        seqEntry.addFeatures([
          {category : 'foo', type : 'bar', start : 5, end : 12, text : 'hello'},
          {category : 'foo', type : 'bar', start : 9, end : 15, text : 'world'}]);
      </script>
    </body>

![Alt text](images/hello-world.jpg "The rendered features. In practice, the widget is zoomable")

##How does it works?
###Principles
A <code>SeqEntry</code> object contains a sequence, a list of features (and whatever you may find convenient for later display).

###Structure

Features are add to the model (if the view is already instanciated, it will be updated once the features are received, thanks to backbone.js).
A feature is an JavaScript object (a plain hashmap, though), containing several fields

 * **groupSet [optional]:** a group of categories information,allowing to have features with the same category name, to be regrouped at first into the same meta-group;
 * **category:** all features from the same category will be handled in the same layer;
 * **categoryType [optional]:** has not grouping purpose, but allows to define properties among different categories (track height, css);
 * **type:** within a category, features can have different types, offering the possibility for custom render (basic is just a rectangle) or interaction;
 * **start:** starting position (0 is the first amino acid of the sequence);
 * **end:** an inclusive end position;
 * **text [optional]:** text to be be displayed in the default rectangle;
 * Whatever you may need for customized display or interaction.

###Third parties dependencies
pViz library uses [D3](http://d3js.org) for SVG generation, [backbone.js](http://backbonejs.org) framework for the model/view framework, [Twitter Bootstrap](http://getbootstrap.com/) for the css, [underscore.js](http://underscorejs.org) library for convenient utilities, and [require.js](http://requirejs.org) for dependency management.

But do not worry, including the [packaged distribution](dist/pviz-bundle-min.js) bundles all of them for you.

##Examples
Easier than a full documentation, we bring some demonstration use cases:

 * [basic interactive](examples/example-0.html) <sup>**</sup>: 2 categories, a few features and an interactive view;
 * [custom feature display with CSS](examples/example-custom-display-css.html)<sup>*</sup>: css depending on category and feature type;
 * [custom feature display with SVG](examples/example-custom-display.html)<sup>*</sup>: drawing special ideogram depending on the feature type;
 * [different track heights](examples/example-different-track-heights.html)<sup>*</sup>: getting more of less extend tracks depending on the category;
 * [interactive features display](examples/example-interaction.html)<sup>*</sup>: mouse over some feature to see more happen;
 * [DAS source](examples/example-das-reader.html)<sup>*</sup>: loading sequence & features from <a href="http://www.ebi.ac.uk/das-srv/uniprot/das/uniprot">EBI/Uniprot DAS server</a> in an interactive view;
 * [multiple DAS source](examples/example-two-das-reader.html)<sup>*</sup>: ebi/uniprot DAS server is used to populate the sequence, and Pride server to add features and customize the parsing;
 * [peff reader](examples/example-peff-reader.html)<sup>*</sup>: PSI Extended Fasta Format handles classic fasta information plus annotations
 * [one liner](examples/example-one-liner.html)<sup>*</sup>: displaying features in a simple, non interactive ideogram (and potentially showing thousands of them on the same page)
 * [one liner, with multiple categories](examples/example-one-liner-multiple-categories.html)<sup>*</sup>: the same ideogram, but showing multiple categories at once.

<sup>*</sup> download the project locally, at least on your file system, to get javascript executed;

<sup>**</sup> run the example behind a http server (node, apache, lighttp...) because of a ajax JSON query.

##And more
###A few comments on the code
The JavaScript library relies on seom "modern" language components. It is not aimed at running on IE 7.
That said, you can either use the bundled library (with all dependencies) or created you own application using require.js and checked out source code.

####Unit testing
Via jasmine, either in the browser ([test/index.html](test/index.html)) or command line with phantom.js

####Continuous integration
test, distribution etc. can be launched in a CI environment via ant tasks (./build.xml)

###Authors
This library was initiated by 
Alexandre Masselot (masselot.alexandre@gene.com) & Kiran Kiran Mukhyala (mukhyala.kiran@gene.com) within Genentech Bioinformatics & Computation Biology Department.
            
###License
The library is distributed under a BSD license. Full description can be found in [LICENSE.txt](LICENSE.txt)
