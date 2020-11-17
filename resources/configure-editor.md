# How to configure SkoHub Editor with a JSON Schema file

This tutorial shows how to configure input fields and validation with [JSON Schema](https://json-schema.org) for a web form that is built with [SkoHub Editor](https://github.com/skohub-io/skohub-editor).

Prerequisite: any way of publishing a JSON file on the web

## What do I want?

A webform for describing learning resources with JSON-LD as output, fulfilling the following requirements:

- It's JSON-LD, so it has to have a `@context`
- a title and description field that take any string
- a field for the URL of the resource
- a type field selecting a value from a subclass of [schema.org/CreativeWork](https://schema.org/CreativeWork)

## Describing the schema

Starting with a JSON Schema, it is best practice to add some information about the schema itself:

- declare your schema with the [`$schema` keyword](https://json-schema.org/understanding-json-schema/reference/schema.html#schema) by indicating the version of JSON Schema used. As you can see, SkoHub Editor supports JSON Schema version 7.
- add a `title`, `description` and [`unique identifier`](https://json-schema.org/understanding-json-schema/basics.html#declaring-a-unique-identifier) of the schema.
- declaring a unique identifier for the schema with [`$id`](https://json-schema.org/understanding-json-schema/basics.html#id4) is also best practice (which we will skip in the beginning)

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "OER",
    "description": "This is a generic JSON schema for describing an Open Educational Resource with schema.org",
    "type": "object"
}
```

## Add JSON-LD context per default

We want all our documents to be [JSON-LD](https://json-ld.org/) and thus declare a link to an external JSON-LD context that will be added in SkoHub Editor to each document as default: 

```json
{
  "default": {
    "@context": [
      "https://w3id.org/kim/lrmi-profile/draft/context.jsonld",
      {
        "@language": "en"
      }
    ]
}
```

**Note**:
- This does not validate the data for a context but does only add the `@context` object per default to the data in SkoHub Editor. Thus, when opening the editor, the context is already there:
- As you can see, there is also a `@language` definition in the context. By adding the [`@language` JSON-LD keyword]() to the context like this, all strings not tagged otherwise will be language tagged with `en`.

[![Screenshot of a freshly loaded SkoHub Editor](/img/skohub-editor.png)](https://skohub.io/editor/)

## Add fields

All properties of a JSON document (which are trhe fields in SkoHub Editor) are configured in the `properties` object of the JSON Schema.

### Add title & description field

Let's start with name and description:

```json
{
   "properties":{
      "name":{
         "title":"Title",
         "type":"string"
      },
      "description":{
         "title":"Description",
         "description":"A description of the learning resource",
         "type":"string",
         "_display":{
            "rows":5,
            "placeholder":"A short description of the resource"
         }
      }
   }
}
```

These are examples for configuring **a property with a non-specified string as value** in JSON Schema.
It also shows how to add a **placeholder** in the field of a web form and how to configure the **size of the input box for a field** in SkoHub Editor. (Note, that all custom keys that are not part of JSON Schema and were added for the configuration of SkoHub Editor start with an underscore `_`.)

### Add field for URL / `@id`

We need record the URL of a resource and will put it in the `id` field which is mapped to the JSON-LD keyword `@id` in the [context document](https://w3id.org/kim/lrmi-profile/draft/context.jsonld) and, this, will work as subject of the triples when you transform the JSON-LD to other RDF serializations.

```json
"id": {
  "title": "URL",
  "type": "string",
  "format": "uri",
  "_display": {
    "placeholder": "The URL of the resource"
  }
}
```

This is an example of **specifiying a property with formated string** (here URI) with JSON Schema. It also shows how to add a placeholder to a field in SkoHub Editor.←

### Add type field with dropdown for selection

We also need to indicate the type of a resource. For this, we want people to use subtypes of [https://schema.org/CreativeWork](https://schema.org/CreativeWork) so we are adding them with the [`enum`](https://json-schema.org/understanding-json-schema/reference/generic.html#enumerated-values) keyword.

```json
"type":{
  "title":"Type",
  "type":"array",
  "items":{
      "type":"string",
      "enum":[
        "Article",
        "Atlas",
        "Blog",
        "Book",
        "Chapter",
        "Clip",
        "Collection",
        "ComicStory",
        "Comment",
        "Conversation",
        "Course",
        "CreativeWorkSeason",
        "CreativeWorkSeries",
        "DataCatalog",
        "Dataset",
        "DigitalDocument",
        "Drawing",
        "Episode",
        "ExercisePlan",
        "Game",
        "Guide",
        "HowTo",
        "LearningResource",
        "Legislation",
        "Manuscript",
        "Map",
        "MediaObject",
        "Movie",
        "MusicComposition",
        "MusicPlaylist",
        "MusicRecording",
        "Painting",
        "Photograph",
        "Play",
        "Poster",
        "PublicationIssue",
        "PublicationVolume",
        "Quotation",
        "Review",
        "Sculpture",
        "SheetMusic",
        "ShortStory",
        "SoftwareApplication",
        "SoftwareSourceCode",
        "TVSeason",
        "TVSeries",
        "VisualArtwork",
        "WebContent",
        "WebPage",
        "WebPageElement",
        "WebSite"
      ]
  }
}
```

### Assign subject

Of course we also want to assign one of the [school subjects vocabulary]() to the resource that we defined in the [previous tutorial](https://github.com/skohub-io/swib20-workshop/tree/main/resources#4-publishing-a-small-controlled-vocabulary-with-skohub-vocabs). As property, we will use [schema.org/about](https://schema.org/about). The respective JSON Schema configuration looks like this:

```json
"about":{
  "title":"Subject",
  "description":"The school subject this learning resource was created for.",
  "type":"object",
  "properties":{
      "type":{
        "type":"string",
        "enum":[
            "Concept"
        ]
      },
      "id":{
        "type":"string",
        "format":"uri",
        "pattern":"^https:\/\/w3id.org\/acka47\/subjects\/.*"
      },
      "inScheme":{
        "type":"object",
        "properties":{
            "id":{
              "type":"string",
              "enum":[
                  "https://w3id.org/acka47/subjects/"
              ]
            }
        }
      },
      "prefLabel":{
        "title":"The preferred label of the concept",
        "description":"A localized string for prefLabel of a SKOS concept",
        "$ref":"https://dini-ag-kim.github.io/lrmi-profile/draft/schemas/LocalizedString.json"
      }
  },
  "required":[
      "id"
  ],
  "_widget":"SkohubLookup"
}
```

## The result

The whole schema is published at [https://github.com/acka47/skohub-example-json-schema/blob/main/schema.json](https://github.com/acka47/skohub-example-json-schema/blob/main/schema.json). You can load the SkoHub Editor based on this schema using the [URL of the raw file](https://raw.githubusercontent.com/acka47/skohub-example-json-schema/main/schema.json) with the `schema` URL parameter like this:

https://skohub.io/editor/?schema=https://raw.githubusercontent.com/acka47/skohub-example-json-schema/main/schema.json

## Splitting a schema into several files 

A schema can be split up into many files. There can be several reasons for this:

- to improve readibility and add structure which makes sense especially for bigger schemas
- to enable schema reuse per field

Latest at this point we have to add unique identifiers to each part of the schema so that they can reference each other. Ideally, we publish the parts of the schema on the web (e.g. with GitHub Pages or similar) and use the URL as `$id`. See the split-up version of the example from this tutorial as published with GitHub Pages: https://acka47.github.io/skohub-example-json-schema/split-up/schema.json. As you can see it just looks the same when loaded in SkoHub Editor: https://skohub.io/editor/?schema=https://acka47.github.io/skohub-example-json-schema/split-up/schema.json