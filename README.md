# Find the right spot in your 4D Write Pro document with AI testgit

In 4D applications, large documents are commonplace: financial reports, internal guidelines, technical manuals… Searching for an exact keyword often isn’t enough. Scrolling through 30-page reports to find one paragraph is not only time-consuming but also error-prone. This is where AI can help.

The&nbsp;[semantic approach based on vectors](https://blog.4d.com/ai-brings-magical-search-to-4d-write-pro-documents/), introduced in 4D 20 R10, already makes it possible to find a relevant&nbsp;[4D Write Pro](https://us.4d.com/4D-write-pro)&nbsp;document even when different wordings are used (for example, “insert image” vs. “add picture”).

But what happens when a document spans multiple pages and covers various subtopics? Even if the entire text can be converted into a single vector, results are often better when we&nbsp;**work at a finer scale**. This is the idea behind chunking: splitting a document into coherent segments, each represented by its own vector.

This is precisely what allows us to go further: retrieving not only&nbsp;**the right document**, but also&nbsp;**the exact passage**&nbsp;that matches the search.

![](https://blog.4d.com/wp-content/uploads/2025/08/Structure_Documents_Chunks.png)

```4d
$colRange:=WP Get elements($doc.WP; wk type paragraph)
// For each paragraph, create a chunk
For each ($paragraph; $colRange)
 $chunk:=ds.Chunk.new()
 $chunk.ID_Document:=$doc.ID
 $chunk.startOffset:=WP Paragraph range($paragraph).start
 $chunk.endOffset:=WP Paragraph range($paragraph).end
 $chunk.textExtract:=WP Get text($paragraph)
 // Generate vector embedding using AIManagement
 $chunk.embedding:=cs.AIManagement.new($apiKey).generateVector($chunk.textExtract)
 $chunk.save()
End for each
```

```4d
$queryParts:=New collection("arrivalDate >= :1";"arrivalTime >= :2")  $queryParts.push("arrivalDate