---
layout: default
title: Specification
nav_order: 2
description: "TEASPN: Framework and Protocol for Integrated Writing Assistance Environments"
permalink: /specification
image: logo.png
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
	/**
	 * Text document to highlight
	 **/
	textDocument: TextDocumentIdentifier
}

interface SyntaxHighlight {
	/**
	 * Range to highlight
	 **/
	range: Range
	/**
	 * Type of highlight. Currently, 'blue', 'red', 'coral', 'cyan', 'green', 'salmon',
	 * 'seagreen', 'yellow', 'orange', 'gold', 'lime', 'skyblue' are defined
	 **/
	type: string
	/**
	 * Message to show when hovered
	 */
	hoverMessage?: string
}
```

## Grammatical Error Detection

Typological/grammatical errors are detected and pushed from the server to the client. This is often (but not necessarily) triggered by requests such as `textDocument/didOpen` and `textDocument/didChange`. 

In order to push the result of typological/grammatical analysis, use the `textDocument/publishDiagnostics` notification. Its specification follows [that of LSP](https://microsoft.github.io/language-server-protocol/specification#textDocument_publishDiagnostics).

## Grammatical Error Correction

Correction of typological/grammatical errors published by the notification mentioned above can be done through the [`textDocument/codeAction`](https://microsoft.github.io/language-server-protocol/specification#textDocument_codeAction) request. When the code action request is issued from the client with regard to a diagnostic (an error detected above), the request parameter contains the relevant diagnostic in the `context` field (of type `CodeActionContext`). A TEASPN server can respond by creating a list of possible `CodeAction`s corresponding to the diagnostic, which are then presented to the user on the client side.

Once the user chooses an action, another request `workspace/executeCommand` is issued from the client. The content of the command is exactly what the `CodeAction` created above contains as the `command` field. The current TEASPN protocol uses the following command for resolving diagnostics:

```typescript
interface Command {
	/**
	 * Human-readable name of the command, e.g., "Quick fix: typo"
	 */
	title: string;
	/**
	 * The identifier of the command. This needs to be "refactor.rewrite"
	 */
	command: string;
	/**
	 * Arguments that the command handler should be
	 * invoked with. For "refactor.rewrite", this is a single-element array
	 * which contains WorkspaceEdit.
	 */
	arguments?: any[];
}

```

Upon receiving the `workspace/executeCommand`, the server issues a `workspace/applyEdit` request to make the actual edit to the documents.

## Completion

Completion (or suggestion) is handled by the [`textDocument/completion`](https://microsoft.github.io/language-server-protocol/specification#textDocument_completion) request. When completion is invoked on the client side (either manually or automatically), the request is sent to the server with the current cursor `Position`. The server needs to reconstruct the context of completion (for example by extracting the partial word at the cursor) and returns a `CompletionList`, which is a list of `CompletionItem`s.

## Text Rewriting

Text rewriting is a general framework where the TEASPN server receives a range in the document and returns another text. The potential uses include, but not limited to:

* Paraphrasing
* Translation
* Summarization
* Search

As with grammatical error correction, text rewriting is handled through the [`textDocument/codeAction`](https://microsoft.github.io/language-server-protocol/specification#textDocument_codeAction) request. The request is sent from the client to the server but *without* the `context` information. The server executes whatever text rewriting operations it wishes and returns a list of `CodeAction`s to the client. The rest of the process is similar to that of grammatical error correction.

## Example Search

Writers often search certain words and phrases on dictionaries and search engines to look for better expressions or definitions. The TEASPN protocol has built-in support for full text search of outside resources. To run search, the client issues a `workspace/searchExample` request, as defined below:

*Request*:

* method: `workspace/searchExample`
* params: `WorkspaceSearchExampleParams` as defined below:

```typescript
interface WorkspaceSearchExampleParams {
	/**
	 * Query string to search with
	 **/
	query: string;
}
```

*Response*:

* result: `ExampleInformation[]` as defined below:

```typescript
interface ExampleInformation {
	/**
     * Label of the example. This is usually the main text of a search result item
	 **/
	label: string;
	/**
	 * Description of the example. This is used as supplementary information (e.g., translation)
	 */
	description: string;
}
```

## Reference Jump

Major IDEs for programming provides the "go to definition" feature where you can jump to the definition by clicking an identifier (such as variables or functions).

Similarly, TEASPN provides the reference jump feature where certain words can be "linked" to other locations in the document. This can be used for cases like:

* Jumping to the referent from a pronoun
* Jumping to the first mention given a proper noun
* Jumping to the definition given an acronym

For this, we re-purpose the [`textDocument/definition`](https://microsoft.github.io/language-server-protocol/specification#textDocument_definition) request from LSP, which returns a list of `Location`s given a `Position` in the document. 

## Mouse Hover

When the writer hovers the mouse cursor over certain locations, a [`textDocument/hover`](https://microsoft.github.io/language-server-protocol/specification#textDocument_hover) request is sent from the client to the server. The server computes related information about the requested location and returns it to the client, which in turn typically displays it as a tooltip. This can be used for, e.g., showing the definition given a word, among other uses.
