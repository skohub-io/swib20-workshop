# SkoHub Editor

The [editor](https://skohub.io/editor/) is used for describing resources on the web and working closley together with the vocabs tool.
It is also capable of sending notifications based on the [Activity Pub Protocol](https://www.w3.org/TR/activitypub/).

## Install

To run the editor locally, just follow the instructions in the README at the [editors GitHub repository](https://github.com/skohub-io/skohub-editor).

## Defining fields in the editor

The editor uses a [JSON Schema](https://json-schema.org/) file to define its fields and the expected input type (you find it in the head bar of the editor).
You can see an exemplary JSON Schema file [here](https://dini-ag-kim.github.io/lrmi-profile/draft/schemas/schema.json).
We can also define a so called "SkoHub Lookup" in the JSON Schema file and point to a vocabulary published with skohub-vocabs.
The items of the vocabulary will then automatically be pulled in the editor in the respective field.
This ensures your editor is always up to date with the latest version of the linked vocabulary.
In a later video we will show, how to set up a JSON Schema file by yourself.


## Sending Activity Pub notifications

If skohub-vocabs is connected the [skohub-pubsub](https://github.com/skohub-io/skohub-pubsub)-module, another module of the skohub components each item of the vocabulary will become an [Activity Pub Actor](https://www.w3.org/TR/activitypub/#actors) and thus get its own [inbox](https://www.w3.org/TR/activitypub/#inbox), where we can send metadata, if we click on the publish button in the skohub-editor.
See [here](https://skohub.io/inbox?actor=dini-ag-kim%2Fhochschulfaechersystematik%2Fheads%2Fmaster%2Fw3id.org%2Fkim%2Fhochschulfaechersystematik%2Fn271) for such an inbox.

This inbox can be followed by each human and machine working and understanding Activity Pub, e.g. Mastodon-based platforms like <https://openbiblio.social/>.
Just enter the URI of the vocabulary in the Mastodon searchbar and a bot created by skohub-pubsub will be found.

## SkoHub-Extension

The [skohub-extension](https://github.com/skohub-io/skohub-extension) is a browser extension, which embeds the editor in Firefox or Chrome.
When you click the icon, a sidebar will open and the correct url of the page you are currently on and some other available metadata will automatically be pulled into the respective fields.

Again, if you click publish and some skohub-vocabs related vocabulary was selected, the metadata will be sent to the items inbox.
