# MP3 Reflection

## Grounding and abstention

The exact-date question about when Vincent Spaulding was hired is a useful
abstention test. The story only says he had been working for Jabez Wilson for
about a month before the Red-Headed League advertisement appeared; it does not
give a calendar date. A specific date would therefore be unsupported. The
assistant uses a default retrieval threshold of 0.30, but this question is also
a reminder that a high retrieval score is not proof that the retrieved text
contains the requested fact. The threshold should be judged against the eval
results, rather than treated as a guarantee of factual support.

## What “Irene Adler Wins” means

I asked, “What does the heading ‘Irene Adler Wins’ mean?” The heading itself is
metadata, while the explanation is spread across the passages describing
Adler's letter and Holmes's reaction. Initially, the assistant abstained even
though retrieval found relevant excerpts: the top score was 0.646. This showed
that retrieval and generation are separate failure points: relevant context
can be retrieved, but the model may still fail to interpret the question as
asking for an explanation of the heading. I updated the prompt to tell the
assistant to explain headings from their supporting excerpts and allow directly
supported inferences. After that change, the assistant answered with citations.

The heading means Adler outsmarted Holmes. She recognized his disguises, left
with the photograph, and promised not to use it against the King. Holmes had
failed to recover the photograph and admired her cleverness, calling her “THE
woman.” This question helped identify that section headings should be included
in the embedding text and that the prompt should explicitly support explaining
what those headings mean. After changing embedding input, the collection must
be re-ingested so Qdrant uses vectors made from the updated text.

## How John Clay trapped Jabez Wilson

The question “How did John Clay trap Jabez Wilson as part of his plan?” tests
whether the answer connects details rather than just retrieving a character
name. Clay used the Red-Headed League as a deception: it kept Wilson away from
his pawnshop for four hours each day. While Wilson was occupied elsewhere, Clay
could dig a tunnel from the cellar toward the City and Suburban Bank vault. A
grounded answer should explain both the distraction and what it enabled, and
cite the Red-Headed League excerpt. This is a useful multi-step question because
mentioning only the League or only the tunnel would leave out how the plan
worked.

## Citations and evaluation

For the Irene Adler question, the answer cited multiple retrieved passages,
which matters because the explanation depends on several events. The citation
validator checks that citation numbers refer to retrieved chunks; it cannot by
itself establish that a cited passage truly supports every claim. The John Clay
question likewise needs evidence for both the diversion and the tunnel.


## One surprise

The most useful surprise was that the Irene Adler query retrieved relevant
chunks with a strong score and still received an abstention. Inspecting both
the retrieved excerpts and the generated response made it clear that the issue
was not simply “the search failed.” Adding section headings to embedding input
and clarifying how to interpret a heading addressed different parts of the
pipeline, and re-ingestion was necessary for the embedding change to take effect.
