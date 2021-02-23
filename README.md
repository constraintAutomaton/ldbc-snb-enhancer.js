# LDBC SNB Enhancer

[![Build Status](https://travis-ci.com/rubensworks/ldbc-snb-enhancer.js.svg?branch=master)](https://travis-ci.com/rubensworks/ldbc-snb-enhancer.js)
[![Coverage Status](https://coveralls.io/repos/github/rubensworks/ldbc-snb-enhancer.js/badge.svg?branch=master)](https://coveralls.io/github/rubensworks/ldbc-snb-enhancer.js?branch=master)
[![npm version](https://badge.fury.io/js/ldbc-snb-enhancer.svg)](https://www.npmjs.com/package/ldbc-snb-enhancer)

Generates auxiliary data based on an [LDBC SNB](https://github.com/ldbc/ldbc_snb_datagen) social network dataset.

## Installation

```bash
$ npm install -g ldbc-snb-enhancer
```
or
```bash
$ yarn global add ldbc-snb-enhancer
```

## Usage

### Invoke from the command line

This tool can be used on the command line as `ldbc-snb-enhancer`,
which takes as single parameter the path to a config file:

```bash
$ ldbc-snb-enhancer path/to/config.json
```

### Config file

The config file that should be passed to the command line tool has the following JSON structure:

```json
{
  "@context": "https://linkedsoftwaredependencies.org/bundles/npm/ldbc-snb-enhancer/^1.0.0/components/context.jsonld",
  "@id": "urn:ldbc-snb-enhancer:default",
  "@type": "Enhancer",
  "Enhancer:_options_personsPath": "path/to/social_network_person_0_0.ttl",
  "Enhancer:_options_destinationPath": "path/to/social_network_auxiliary.ttl",
  "Enhancer:_options_logger": {
    "@type": "LoggerStdout"
  },
  "Enhancer:_options_dataSelector": {
    "@type": "DataSelectorRandom",
    "DataSelectorRandom:_seed": 12345
  },
  "Enhancer:_options_handlers": [
    {
      "@type": "EnhancementHandlerPosts",
      "EnhancementHandlerPosts:_chance": 0.3
    }
  ]
}
```

The important parts in this config file are:
* `"Enhancer:_options_personsPath"`: Path to the persons output file of [LDBC SNB](https://github.com/ldbc/ldbc_snb_datagen).
* `"Enhancer:_options_destinationPath"`: Path of the destination file to create.
* `"Enhancer:_options_logger"`: An optional logger for tracking the generation process. (`LoggerStdout` prints to standard output)
* `"Enhancer:_options_dataSelector"`: A strategy for selecting values from a collection. (`DataSelectorRandom` selects random values based on a given seed)
* `"Enhancer:_options_handlers"`: An array of enhancement handlers, which are strategies for generating data.

## Configure

### Handlers

The following handlers can be configured.

#### Posts Handler

Generate posts and assign them to existing people.

```json
{
  "Enhancer:_options_handlers": [
    {
      "@type": "EnhancementHandlerPosts",
      "EnhancementHandlerPosts:_chance": 0.3
    }
  ]
}
```

Parameters:
* `"EnhancementHandlerPosts:_chance"`: The chance for posts to be generated. The number of posts will be the number of people times this chance, where people are randomly assigned to posts.

Generated shape:
```turtle
<http://www.ldbc.eu/ldbc_socialnet/1.0/data/post-fake2045> a <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/Post>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/id> "2045";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/fake> "true";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/hasCreator> <http://www.ldbc.eu/ldbc_socialnet/1.0/data/pers00000021990232555524>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/creationDate> "2021-02-22T10:39:31.595Z";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/locationIP> "200.200.200.200";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/browserUsed> "Firefox";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/content> "Tomatoes are blue";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/length> "17";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/language> "en";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/locatedIn> <http://dbpedia.org/resource/Belgium>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/hasTag> <http://www.ldbc.eu/ldbc_socialnet/1.0/tag/Georges_Bizet>.
```

#### Post Contents Handler

Generate additional contents for existing posts.

```json
{
  "Enhancer:_options_handlers": [
    {
      "@type": "EnhancementHandlerPostContents",
      "EnhancementHandlerPostContents:_chance": 0.3
    }
  ]
}
```

Parameters:
* `"EnhancementHandlerPostContents:_chance"`: The chance for post content to be generated. The number of new post contents will be the number of posts times this chance, where contents are randomly assigned to posts. @range {double}

Generated shape:
```turtle
<http://www.ldbc.eu/ldbc_socialnet/1.0/data/post-fake2045> a <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/Post>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/id> "2045";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/fake> "true";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/hasCreator> <http://www.ldbc.eu/ldbc_socialnet/1.0/data/pers00000021990232555524>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/creationDate> "2021-02-22T10:39:31.595Z";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/locationIP> "200.200.200.200";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/browserUsed> "Firefox";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/content> "Tomatoes are blue";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/length> "17";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/language> "en";
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/locatedIn> <http://dbpedia.org/resource/Belgium>;
    <http://www.ldbc.eu/ldbc_socialnet/1.0/vocabulary/hasTag> <http://www.ldbc.eu/ldbc_socialnet/1.0/tag/Georges_Bizet>.
```

#### Post Authors Handler

Generate additional authors for existing posts.

```json
{
  "Enhancer:_options_handlers": [
    {
      "@type": "EnhancementHandlerPostAuthors",
      "EnhancementHandlerPostAuthors:_chance": 0.3
    }
  ]
}
```

Parameters:
* `"EnhancementHandlerPostAuthors:_chance"`: The chance for a post author to be generated. The number of new post authors will be the number of posts times this chance, where authors are randomly assigned to posts.

## License

This software is written by [Ruben Taelman](http://rubensworks.net/).

This code is released under the [MIT license](http://opensource.org/licenses/MIT).
