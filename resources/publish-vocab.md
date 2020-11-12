# How to publish a SKOS vocabulary with SkoHub (Docker version)

This tutorial shows how to create and publish a SKOS vocabulary using the [Docker version of SkoHub Vocabs](https://github.com/skohub-io/skohub-vocabs/tree/docker-gh-pages) .  

Prerequisite: GitHub account

## Scenario

In this exercise, we set up an example controlled vocabulary with SkoHub Vocabs to classify learning material by school subject.

## Step 1: Set up your skohub-docker-vocabs repository

See [https://github.com/skohub-io/swib20-workshop/tree/main/resources#3-using-skohub-docker-version](https://github.com/skohub-io/swib20-workshop/tree/main/resources#3-using-skohub-docker-version) for a detailed walkthrough.

## Step 2: Define your basic vocabulary

For a start, I need a basic list of school subjects: maths, english, science, history, geography, physics, chemistry, physical education, computerScience, biology, foreign languages

## Step 3: Make it SKOS

- Use an already functioning vocabulary as template, e.g. the [example vocab created for this tutorial](https://github.com/acka47/skohub-docker-vocabs/blob/master/subjects.ttl).
- Decisions to make:
    - Hash or slash URIs? 
    - Should I use a persistent identifier scheme? If yes, which one: [w3id.org](https://w3id.org/), [PURL](http://purl.org)?

## Step 4: Validate & push to GitHub

- There are lots of options for validating RDF:
    - cli tools like [rapper](http://librdf.org/raptor/rapper.html) or [riot](https://jena.apache.org/documentation/io/)
    - Web-based tools like [RDF translator](https://rdf-translator.appspot.com/)
- Even better: validate SKOS, e.g. by using the web-based [SKOS testing tool](https://labs.sparna.fr/skos-testing-tool/)

# Step 5: Add some hierarchy to the vocabulary

There is some granularity missing with "foreign languages". I want to add specific languages to better classify the material. We use `skos:broader`/`skos:narrower` to add sub-concepts to `foreignLanguages`.

# Step 6: Set up redirect for persistent identifiers

If you are using PURL or w3id, you have to set up the redirect so that the permanent URIs resolve.

# Step 6: Publish and use it

Now you can use the concept URIs when describing a resource. As SkoHub Vocabs provides a [FlexSearch](https://github.com/nextapps-de/flexsearch) index  (to view it just use the file extension `.index` with the vocab URL, e.g. https://acka47.github.io/skohub-docker-vocabs/purl.org/acka47/subjects.index), you can easily integrate a lookup in every website. The SkoHub Editor – which will be covered in the next tutorial – also let's you configure SKOS lookup for a field in a web form.
