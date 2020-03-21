# Capítulo 6: Aggregation Performance y Pipeline Optimization

### Lecciones

* Tema: Aggregation Performance
* Examen
* Tema: Aggregation Pipeline en un Sharded Cluster
* Examen
* Tema: Pipeline Optimization - Parte 1
* Tema: Pipeline Optimization - Parte 2
* Examen

## 1. Tema: Aggregation Performance

### Transcripción

## 2. Examen Aggregation Performance

**Problem:**

With regards to aggregation performance, which of the following are true?

Check all answers that apply:

* You can increase index usage by moving `$match` stages to the end of your pipeline

* Passing `allowDiskUsage` to your aggregation queries will seriously increase their performance

* When `$limit` and `$sort` are close together a very performant top-k sort can be performed

* Transforming data in a pipeline stage prevents us from using indexes in the stages that follow

## 3. Tema: Aggregation Pipeline en un Sharded Cluster

### Notas de lectura

**Nota**: A las 3:05, las dos etapas `$limit` se fusionarán en una etapa de límite de `$limit: 5`, no de `$limit: 15` como se muestra en el video.

Puede obtener más información sobre la agregación en un clúster fragmentado visitando la sección [Aggregation Pipeline and Sharded Collections](https://docs.mongodb.com/manual/core/aggregation-pipeline-sharded-collections/?jmp=university) del Manual MongoDB.

### Transcripción

## 4. Examen Aggregation Pipeline on a Sharded Cluster

**Problem:**

What operators will cause a merge stage on the primary shard for a database?

Check all answers that apply:

* `$group`

* `$lookup`

* `$out`

## 5. Tema: Pipeline Optimization - Parte 1

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 6. Tema: Pipeline Optimization - Parte 2

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 7. Examen Pipeline Optimization - Part 2

**Problem:**

Which of the following statements is/are true?

Check all answers that apply:

* The query in a `$match` stage can be entirely covered by an index

* The Aggregation Framework will automatically reorder stages in certain conditions

* Causing a merge in a sharded deployment will cause all subsequent pipeline stages to be performed in the same location as the merge

* The Aggregation Framework can automatically project fields if the shape of the final document is only dependent upon those fields in the input document.
