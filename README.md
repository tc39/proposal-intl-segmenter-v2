# Intl Segmenter v2 Proposal

Intl.Segmenter v2: Unicode segmentation in JavaScript

A repository template for ECMA402 proposals.

You can browse the [ecmarkup output](https://FrankYFTang.github.io/proposal-intl-segmenter-v2/)
or browse the [source](https://github.com/FrankYFTang/proposal-intl-segmenter-v2/blob/HEAD/spec.emu).

## Stage 0

## Stage Advancement Slide
* Slides for TG2 Stage 1 concensus TBW (Plan to discuss in ECMA402 TG2 2021 Nov 4 meeting)
* Slides for Stage 1 Advancement TBW (Plan propose to Stage 1 in TC39 2021 Dec 15 meeting)

## Staff
* Champion: Frank Yung-Fong Tang
* Stage 3 Reviewers (To be requested/signed up while advancing to Stage 2):
  * TBD

## Motivation
This proposal aim to improve the [Intl.Segmenter](https://tc39.es/proposal-intl-segmenter/) API with:
* A way to segment the text with a batch call to minimize the number of calls between user calls and internal implementation.
* Add line break granularity to support high end ECMAScript based text layout applications which already have a way to measure the text width but need a way to access logical line break points.

## Use Cases
### Batch Mode

TBW

### Line Break Granularity
* (From @gibson042 ) Formatting in a monospace context, such as a CLI or GitHub source diff.
* For non-HTML context while font metrics is available 
* &lt;text&gt; in SVG and https://d3js.org/ 
* To avoid the misue of word break in the place for line break. (Canvas drawstring and SVG usecase)
* Being persistently requested by users even after the shipment of Intl.Segmenter



## Examples
TBW

## [TODO before Stage 1](https://tc39.es/process-document/)
### Entrance Criteria to Stage 1
* Identified “champion” who will advance the addition **DONE => Frank Yung-fong Tang**
* Prose outlining the problem or need and the general shape of a solution
* Illustrative examples of usage
* High-level API
* Discussion of key algorithms, abstractions and semantics
* Identification of potential “cross-cutting” concerns and implementation challenges/complexity
* A publicly available repository for the proposal that captures the above requirements **DONE => this one**

### Acceptance Signifies for Stage 1 
The committee expects to devote time to examining the problem space, solutions and cross-cutting concerns

### Purpose During Stage 1
* Make the case for the addition
* Describe the shape of a solution
* Identify potential challenges

## [TODO before Stage 2](https://tc39.es/process-document/)
### Entrance Criteria to Stage 2
...
### Acceptance Signifies for Stage 2
...
### Purpose During Stage 2
...

### Analysis of TG2 Requirements
#### Prior Art (for Stage 2)
TBW

#### Expensive to Implement in Userland (for Stage 2)
TBW

#### Broad Appeal (for Stage 2)
TBW

## [TODO before Stage 3](https://tc39.es/process-document/)
### Entrance Criteria to Stage 3
...
### Acceptance Signifies for Stage 3
...
### Purpose During Stage 3
...
### Analysis of TG2 Requirements
#### Payload Mitigation (for Stage 3)
TBW
      
# TODO of the repo
 
  1.  Avoid merge conflicts with build process output files by running:
      ```sh
      git config --local --add merge.output.driver true
      git config --local --add merge.output.driver true
      ```
  1.  Add a post-rewrite git hook to auto-rebuild the output on every commit:
      ```sh
      cp hooks/post-rewrite .git/hooks/post-rewrite
      chmod +x .git/hooks/post-rewrite
      ```
  1. changes to `spec.emu` (ecmarkup uses HTML syntax, but is not HTML, so I strongly suggest not naming it ".html")
  1. Any commit that makes meaningful changes to the spec, should run `npm run build` and commit the resulting output.
  1. Whenever you update `ecmarkup`, run `npm run build` and commit any changes that come from that dependency.

  [explainer]: https://github.com/tc39/how-we-work/blob/HEAD/explainer.md
