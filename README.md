# Intl Segmenter v2 Proposal

Intl.Segmenter v2: Unicode segmentation in JavaScript

A repository template for ECMA402 proposals.

You can browse the [ecmarkup output](https://FrankYFTang.github.io/proposal-intl-segmenter-v2/)
or browse the [source](https://github.com/FrankYFTang/proposal-intl-segmenter-v2/blob/HEAD/spec.emu).

## Stage 0
[Proposed Spec Text (in green) as diff of Intl.Segmenter](https://frankyftang.github.io/proposal-intl-segmenter-v2/)

## Stage Advancement Slide
* [Slides for TG2/ECMA402 Stage 1 concensus](https://docs.google.com/presentation/d/1ezpdee0r_ujHXDqqT4HHNYa2Q4VU7b4cMQWg6gQxLTk/) (Plan to discuss in TG2/ECMA402 2021 Dec 2 meeting)
* [Slides for TC39 Stage 1 Advancement](https://docs.google.com/presentation/d/1ezpdee0r_ujHXDqqT4HHNYa2Q4VU7b4cMQWg6gQxLTk/) (Plan propose to Stage 1 in TC39 2021 Dec 15 meeting)

## Staff
* Champion: Frank Yung-Fong Tang
* Stage 3 Reviewers (To be requested/signed up while advancing to Stage 2):
  * TBD

## Motivation
This proposal aim to improve the [Intl.Segmenter](https://tc39.es/proposal-intl-segmenter/) API with:
* ~~A way to segment the text with a batch call to minimize the number of calls between user calls and internal implementation.~~
* Add line break granularity to support high end ECMAScript based text layout applications which already have a way to measure the text width but need a way to access logical line break points.

## Use Cases
### ~~Batch Mode~~

Note: The "Batch Mode" is removed from the proposal after careful discussion with some intended users. 
### Line Break Granularity
* **From @gibson042:** Formatting in a monospace context, such as a CLI or GitHub source diff.
* For non-HTML context while font metrics is available 
* &lt;text&gt; in SVG and https://d3js.org/ 
* To avoid the misue of word break in the place for line break. (Canvas drawstring and SVG usecase)
* Being persistently requested by users even after the shipment of Intl.Segmenter
* **From @my2iu :** It is becoming increasingly common now for web apps to have custom text rendering, and these web apps need an API for line segmentation to be able to lay out text in lines with wrapping. HTML/CSS are too high-level to provide the font support that many web apps need, so low-level API is needed. Line-breaking libraries tend to be fairly large, so it's not really practical to directly bundle them in web apps either, so it would be better if the browser could provide this functionality. Custom text rendering is used in
  * application web apps for word processing, vector illustration, paint programs, svg visualizations, etc any web page that needs to render text in WebGL or VR/AR (via WebXR). 
  * Good-looking text in 3d engines is commonly implemented using signed-distance fields, which requires a custom text renderer
* **From @HackbrettXXX , contributor to jsPDF**:
  * Creating text documents of any kind: e.g. PDF with jsPDF: jsPDF offers functionality to automatically split text into lines. However, currently it only supports Western-style line breaks: words = text.split(" "). Adding Line Break to Intl.Segmenter could improve that. In jsPDF, the text is measured by loading the font files and calculating the text size from the glyph sizes.
  * Rendering multiline text in a context in the browser where HTML/CSS layout cannot be used, e.g. when using SVG, canvas, or WebGL for rendering. Here, the text can be measured with canvas' measureText() or SVG's getComputedTextLength() or getBBox().
* **From @mdebbar , contributor to Flutter Web **:
  * Needed for [Text layout in Flutter Web](https://gist.github.com/mdebbar/93886d22a4cd40e050d1533d7e0016bf#1-line-breaks).

## Examples
```
d8> s = new Intl.Segmenter("en", {granularity: "line"})
[object Intl.Segmenter]
d8> ss = s.segment("飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動")
{}
d8> for (s of ss) { print(JSON.stringify(s)) }
{"segment":"飛","index":0,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"虎","index":1,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"隊","index":2,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"正","index":3,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"式","index":4,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"名","index":5,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"稱","index":6,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"為","index":7,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"「中","index":8,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"華","index":10,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"民","index":11,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"國","index":12,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"空","index":13,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"軍","index":14,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"美","index":15,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"籍","index":16,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"志","index":17,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"願","index":18,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"大","index":19,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"隊」，\n","index":20,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":true}
{"segment":"1940 ","index":24,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"年","index":29,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"代","index":30,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"早","index":31,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"期","index":32,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"在","index":33,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"緬","index":34,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"甸","index":35,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"展","index":36,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"開","index":37,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"行","index":38,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
{"segment":"動","index":39,"input":"飛虎隊正式名稱為「中華民國空軍美籍志願大隊」，\n1940 年代早期在緬甸展開行動","isHardBreak":false}
d8> ss = s.segment("The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s")
{}
d8> for (s of ss) { print(JSON.stringify(s)) }
{"segment":"The ","index":0,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Flying ","index":4,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Tigers ","index":11,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"is ","index":18,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"officially ","index":21,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"called ","index":32,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"the ","index":39,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"\"Republic ","index":43,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"of ","index":53,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"China ","index":56,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Air ","index":62,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Force ","index":66,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"American ","index":72,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Volunteer ","index":81,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Brigade\",\n","index":91,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":true}
{"segment":"and ","index":101,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"it ","index":105,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"launched ","index":108,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"operations ","index":117,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"in ","index":128,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"Myanmar ","index":131,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"in ","index":139,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"the ","index":142,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"early ","index":146,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
{"segment":"1940s","index":152,"input":"The Flying Tigers is officially called the \"Republic of China Air Force American Volunteer Brigade\",\nand it launched operations in Myanmar in the early 1940s","isHardBreak":false}
```

The above result are based on the real output of [v8 prototype CL](https://chromium-review.googlesource.com/c/v8/v8/+/3324785)

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
