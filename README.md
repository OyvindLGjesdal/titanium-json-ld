> [!IMPORTANT]
>  **This Open-Source Project Needs Your Support**
>
> This library has been provided as a free, open-source resource, benefiting countless users without requiring anything in return. Yet, maintaining it takes significant time, effort, and resources—something often overlooked by those who use it freely.
>
> There is a noticeable gap between usage and contributions. This highlights a challenging reality for open-source: heavy reliance on the work of developers and maintainers without adequate support in return. Open-source projects like this depend on the contributions of their users to remain sustainable.
>
> **Open-source is not free labor. If your company profits from this library but refuses to contribute back, you’re not supporting the ecosystem—you’re exploiting it**.
> 
> This library has been sustained through countless hours of work and resources, provided in good faith to the community. If your business depends on it but doesn’t give back, consider whether you deserve the benefits you’re taking. Support the work, or step aside for those who value collaboration and fairness.
>
>Don’t let open-source be taken for granted. Support the work that supports you.

# Titanium JSON-LD 1.1 Processor & API

An implementation of the [JSON-LD 1.1](https://www.w3.org/TR/json-ld/) (JSON-based Serialization for Linked Data) specification in Java utilizing [Jakarta JSON Processing](https://github.com/eclipse-ee4j/jsonp).

### Goals
- conformance to the specification
- secure, stable, fast, A+ code (covered by **~1800 tests**)
- minimal external dependencies
  - only `jakarta.json-api` is required
- simple to use

### Status

[![Java 11 CI](https://github.com/filip26/titanium-json-ld/actions/workflows/java11-build.yml/badge.svg)](https://github.com/filip26/titanium-json-ld/actions/workflows/java11-build.yml)
[![Android (Java 8) CI](https://github.com/filip26/titanium-json-ld/actions/workflows/java8-build.yml/badge.svg)](https://github.com/filip26/titanium-json-ld/actions/workflows/java8-build.yml)
[![CodeQL](https://github.com/filip26/titanium-json-ld/actions/workflows/codeql.yml/badge.svg)](https://github.com/filip26/titanium-json-ld/actions/workflows/codeql.yml)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/c530c6b43b0243c08ce81521c5b4cf6a)](https://app.codacy.com/gh/filip26/titanium-json-ld/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)
[![Codacy Badge](https://app.codacy.com/project/badge/Coverage/c530c6b43b0243c08ce81521c5b4cf6a)](https://app.codacy.com/gh/filip26/titanium-json-ld/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_coverage)
[![Maven Central](https://img.shields.io/maven-central/v/com.apicatalog/titanium-json-ld.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:com.apicatalog%20AND%20a:titanium-json-ld)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

### Extensions

- [LD-CLI](https://github.com/filip26/ld-cli) is a native command line utility for Ubuntu, Mac, Windows
- [JSON-LD-star](https://json-ld.github.io/json-ld-star) expansion and compaction built-in support (experimental)
- [Universal RDF Dataset Normalization Algorithm - URDNA2015](https://github.com/simon-greatrix/rdf-urdna)
- [Iridium CBOR-LD](https://github.com/filip26/iridium-cbor-ld) a CBOR-based Processor for Linked Data
- [LEXREX](https://lexrex.web.app/) semantic vocabularies visual builder and manager


## Table of Contents  
- [Conformance](#conformance)
- [Examples](#examples)
- [Installation](#installation)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Resources](#resources)
- [Commercial Support](#commercial-support)

## Conformance

 | Feature | Tests | Pass | Status | Notes |
 | --- | ---: | ---: | ---: | --- |
| [Expansion](https://www.w3.org/TR/json-ld/#expanded-document-form) | 373 |  373 | 100% | |
| [Compaction](https://www.w3.org/TR/json-ld/#compacted-document-form) | 243 | 243 | 100% | |
| [Flattening](https://www.w3.org/TR/json-ld/#flattened-document-form) | 55 | 55 | 100% | |
| [JSON-LD to RDF](https://www.w3.org/TR/json-ld/#relationship-to-rdf) | 453 | 451 | 99.5% | <ul><li>[te075 - @vocab as blank node identifier](https://w3c.github.io/json-ld-api/tests/toRdf-manifest#te075)</li><li>[tli12 - List with bad @base](https://w3c.github.io/json-ld-api/tests/toRdf-manifest#tli12)</li></ul> |
| [RDF to JSON-LD](https://www.w3.org/TR/json-ld/#relationship-to-rdf) | 51 | 51  | 100% | |
| [Framing](https://www.w3.org/TR/json-ld11-framing/#framing) | 89 | 88 | 98.8% | <ul><li>[t0059 - @embed: @last](https://w3c.github.io/json-ld-framing/tests/frame-manifest#t0059)</li></ul> |
| [Remote Document and Context Retrieval](https://www.w3.org/TR/json-ld11-api/#remote-document-and-context-retrieval) | 18 | 17 | 94.4% | <ul><li>[t0013 - HTML document](https://w3c.github.io/json-ld-api/tests/remote-doc-manifest#t0013)</li></ul> |

See [EARL results from the JSON-LD 1.1 Test Suite](https://w3c.github.io/json-ld-api/reports/#subj_Titanium_JSON_LD_Java) for more details.

## Examples

Titanium provides high-level [JsonLd](https://javadoc.io/doc/com.apicatalog/titanium-json-ld/latest/com/apicatalog/jsonld/JsonLd.html) API to interact with the processor.

### Transformations

```javascript

// Expansion
JsonLd.expand("https://w3c.github.io/json-ld-api/tests/expand/0001-in.jsonld")
      .ordered()
      .get();

JsonLd.expand("file:/home/filip/document.json")    // HTTP(S) and File schemes supported
      .context("file:/home/filip/context.jsonld")  // external context
      .get();

// Compaction
JsonLd.compact("https://example/expanded.jsonld", "https://example/context.jsonld")
      .compactToRelative(false)
      .get();

// Flattening
JsonLd.flatten("https://example/document.jsonld").get();

// JSON-LD to RDF
JsonLd.toRdf("https://example/document.jsonld").get();

// RDF to JSON-LD
JsonLd.fromRdf("https://example/document.nq").options(options).get();

// Framing
JsonLd.frame("https://example/document.jsonld", "https://example/frame.jsonld").get();

```

### Local JSON Document

```javascript
Document document = JsonDocument.of(InputStream) or JsonDocument.of(Reader) ...

JsonLd.expand(document).get();

JsonLd.compact(document, contextDocument).get();
...
```

### Processing Timeout [experimental]
A processor gets terminated eventually after a specified time. Please note 
the duration does not cover `DocumentLoader` processing time. 
You have to set-up a read timeout separately.

```javascript
// since 1.4.0
JsonLd.expand(...).timeout(duration)...get();
```

### HTTP Document Loader Timeout
Configure and set a custom HTTP document loader instance.

```javascript
// since 1.4.0 - set read timeout
static DocumentLoader LOADER = HttpLoader.defaultInstance().timeount(Duration.ofSeconds(30));
...
JsonLd.expand(...).loader(LOADER).get();
```

### Document caching
Configure LRU-based cache for loading documents.
The argument determines size of the LRU-cache.
```javascript
// since 1.4.0
JsonLd.toRdf("https://example/document.jsonld").loader(new LRUDocumentCache(loader, capacity)).get();
```

You can share an instance of `LRUDocumentCache` among multiple calls to reuse cached documents.
```javascript
// since 1.4.0
DocumentLoader cachedLoader = new LRUDocumentCache(loader, capacity);

JsonLd.toRdf("https://example/document.jsonld").loader(cachedLoader).get();
JsonLd.toRdf("https://example/another-document.jsonld").loader(cachedLoader).get();
```

### Undefined Terms Processing Policy

Set processing policy on undefined terms. `Ignore` by default.

```javascript
// since 1.4.1
JsonLd.expand(...).undefinedTermsPolicy(Fail|Warn|Ignore).get();
```

## Installation

### Titanium

#### Maven
Java 11+

```xml
<dependency>
    <groupId>com.apicatalog</groupId>
    <artifactId>titanium-json-ld</artifactId>
    <version>1.4.1</version>
</dependency>
```

#### Gradle 
Java 8+, Android API Level >=24

```gradle
implementation("com.apicatalog:titanium-json-ld-jre8:1.4.1")
```

### JSON-P Provider

Add JSON-P provider, if it is not on the classpath already.

#### Maven

```xml
<dependency>
    <groupId>org.glassfish</groupId>
    <artifactId>jakarta.json</artifactId>
    <version>2.0.1</version>
</dependency>
```

#### Gradle

```gradle
implementation("org.glassfish:jakarta.json:2.0.1")
```

## Documentation

[![javadoc](https://javadoc.io/badge2/com.apicatalog/titanium-json-ld/javadoc.svg)](https://javadoc.io/doc/com.apicatalog/titanium-json-ld)


## Contributing

All PR's welcome!

- develop
  - implement a new feature 
  - fix an existing issue
  - improve an existing implementation
- test
  - report a bug
  - implement a test case
- document
  - write javadoc
  - write a tutorial
  - proofread an existing documentation
- promote
  - star, share, the project
  - write an article
- sponsor
  - your requests get top priority
  - you will get a badge

### Building

Fork and clone the project repository.

#### Java 11

```bash
> cd titanium-json-ld
> mvn clean package
```

#### Java 8

```bash
> cd titanium-json-ld
> mvn -f pom_jre8.xml clean package
```

## Resources
- [JSON-LD 1.1](https://www.w3.org/TR/json-ld/)
- [JSON-LD 1.1 Processing Algorithms and API](https://www.w3.org/TR/json-ld-api/)
- [JSON-LD 1.1 Framing](https://www.w3.org/TR/json-ld-framing/)
- [JSON-LD Best Practices](https://w3c.github.io/json-ld-bp/)
- [JSON-LD-star](https://json-ld.github.io/json-ld-star/)
- [JSON-LD Playground](https://json-ld.org/playground/)

## Commercial Support
Commercial support is available at filip26@gmail.com

