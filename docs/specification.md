---
layout: default
title: Specification
nav_order: 2
description: "TEASPN: Framework and Protocol for Integrated Writing Assistance Environments"
permalink: /specification
---

# Specification

## Overview

We mentioned on the [home page]({{ site.baseurl }}{% link index.md %}), TEASPN is a fork of [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/) and shares a large potion of its specification with LSP. Both TEASPN and LSP are based on an HTTP-like base protocol which contains the header and the content parts. The content part follows [JSON-PRC](https://www.jsonrpc.org/) for serializing messages. A message can be one of request, response, or notification.

## Text Synchronization

Document changes are communicated via `textDocument/didOpen` and `textDocument/didChange` notifications from the client to the server. By default, TEASPN adopts incremental text synchronization, meaning that after the full content is sent on document open, only the "diffs" of documents are sent from the client. TEASPN servers need to reconstruct the original document internally from incremental updates they receive. 

## Syntax Highlighting

Syntax highlighting is not part of LSP, mainly because it is best done on the client side for most programming languages (See for example [this discussion thread](https://github.com/Microsoft/language-server-protocol/issues/33)). However, lexical and syntactic analyses of natural languages could be too complicated and computationally intensive to be done on the client side.

For this reason, TEASPN supports syntax highlighting on the server side. The syntax highlighting request is sent from the client to the server to request highlighting information for the given document.

*Request*:

* method: `textDocument/syntaxHighlight`
* params: `SyntaxHighlightParams`

*Response*:

* result: `SyntaxHighlight[]` as defined below:

```typescript
interface SyntaxHighlightParams {
	textDocument: TextDocumentIdentifier    // text document to highlight
}

interface SyntaxHighlight {
	range: Range                // range to highlight
	type: string                // one of ["red", "blue"]
	hoverMessage?: string       // message to show when hovered
}
```

## Grammatical Error Detection

Typological/grammatical errors are detected and pushed from the server to the client. This is often (but not necessarily) triggered by requests such as `textDocument/didOpen` and `textDocument/didChange`. 

In order to push the result of typological/grammatical analysis, use the `textDocument/publishDiagnostics` notification. Its specification follows [that of LSP](https://microsoft.github.io/language-server-protocol/specification#textDocument_publishDiagnostics).

## Grammatical Error Correction

TODO

## Completion

TODO

## Text Rewriting

TODO

## Example Search

TODO

## Reference Jump

TODO

## Mouse Hover

TODO
