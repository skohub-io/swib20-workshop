# SkoHub-Vocabs

The [SkoHub-Vocabs](https://github.com/skohub-io/skohub-vocabs) tool can be used to easily publish vocabulary providing one or multiple SKOS-files.
It creates nice HTML-pages as well as machine readable versions, which can then be published using a web server or without any special infrastructure, just using GitHub Pages (though you won't be able to use all the features of skohub with this approach).
You can have a look at some exemplary vocabulary [here](https://skohub.io/dini-ag-kim/hochschulfaechersystematik/heads/master/).

## Installation

To install skohub-vocabs just go to the [GitHub-Repository](https://github.com/skohub-io/skohub-vocabs) and follow the instructions in the README:

### Step 1

Clone the repository:

    $ git clone https://github.com/skohub-io/skohub-vocabs.git


### Step 2

Move into the cloned directory:

    $ cd skohub-vocabs
    
### Step 3

Install the necessary node packages:

    $ npm i
    
### Step 4

Create an environment file out of the example environment file:

    $ cp .env.example .env
    
### Step 5

Copy an example SKOS file to the data folder:

    $ cp test/data/systematik.ttl data/

To build a vocabulary you have to provide one (or multiple) valid SKOS-file in the `data`-folder of skohub-vocabs and run the `npm run build`-command.
This will build a `public`-folder with the `html` and `json` representations of your vocabulary.
This way you get nice human readable and at the same time nice machine readable versions of your vocabulary.

After you made changes to your .env or data/* files, make sure to delete the .cache directory:

    $ rm -rf .cache

You can also watch your built vocabulary locally running skohub-vocabs in development mode:

    $ npm run develop

This will start a development server at: <http://localhost:8000/>

If you are also using the [skohub-pubsub](https://github.com/skohub-io/skohub-pubsub) component, each item of the vocabulary will become an [Activity Pub](https://www.w3.org/TR/activitypub/)-actor and get its own inbox, which you send metadata to and follow on instances supporting Activity Pub (e.g. Mastodon).
