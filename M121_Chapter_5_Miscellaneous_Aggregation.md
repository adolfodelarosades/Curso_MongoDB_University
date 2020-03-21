# Capítulo 5: Miscellaneous Aggregation

Lecciones

* Tema: La etapa `$redact`
* Tema: La etapa `$out`
* Examen
* Tema: Vistas
* Examen

## 1. Tema: La etapa `$redact`

### Notas de lectura

Página de documentación de [`$redact`](https://docs.mongodb.com/manual/reference/operator/aggregation/redact/).

### Transcripción


## 2. Tema: La etapa `$out`

### Transcripción

## 3. Examen The $out Stage

**Problem:**

Which of the following statements is true regarding the `$out` stage?

Choose the best answer:

* `$out` removes all indexes when it overwrites a collection.

* Using `$out` within many sub-piplines of a $facet stage is a quick way to generate many differently shaped collections.

* If a pipeline with `$out` errors, you must delete the collection specified to the `$out` stage.

* `$out` will overwrite an existing collection if specified.

## 4. Tema: Vistas

### Transcripción

## 5. Examen Views

**Problem:**

Which of the following statements are true regarding MongoDB Views?

Choose the best answer:

* View performance can be increased by creating the appropriate indexes on the source collection.

* Inserting data into a view is slow because MongoDB must perform the pipeline in reverse.

* A view cannot be created that contains both horizontal and vertical slices.

* Views should be used cautiously because the documents they contain can grow incredibly large.
