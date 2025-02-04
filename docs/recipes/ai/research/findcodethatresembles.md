---
sidebar_label: "Find method invocations that resemble a pattern"
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Find method invocations that resemble a pattern

**io.moderne.ai.research.FindCodeThatResembles**

_This recipe uses two phase AI approach to find a method invocation that resembles a search string._

## Recipe source

[GitHub](https://github.com/openrewrite/rewrite-ai-search/blob/main/src/main/java/io/moderne/ai/research/FindCodeThatResembles.java), [Issue Tracker](https://github.com/openrewrite/rewrite-ai-search/issues), [Maven Central](https://central.sonatype.com/artifact/org.openrewrite.recipe/rewrite-ai-search/0.19.1/jar)

* groupId: org.openrewrite.recipe
* artifactId: rewrite-ai-search
* version: 0.19.1

## Options

| Type | Name | Description | Example |
| -- | -- | -- | -- |
| `String` | resembles | The text, either a natural language description or a code sample, that you are looking for. | `HTTP request with Content-Type application/json` |
| `int` | k | Since AI based matching has a higher latency than rules based matching, we do a first pass to find the top k methods using embeddings. To narrow the scope, you can specify the top k methods as method filters. | `5` |


## Usage

This recipe has required configuration parameters. Recipes with required configuration parameters cannot be activated directly. To activate this recipe you must create a new recipe which fills in the required parameters. In your `rewrite.yml` create a new recipe with a unique name. For example: `com.yourorg.FindCodeThatResemblesExample`.
Here's how you can define and customize such a recipe within your rewrite.yml:
```yaml title="rewrite.yml"
---
type: specs.openrewrite.org/v1beta/recipe
name: com.yourorg.FindCodeThatResemblesExample
displayName: Find method invocations that resemble a pattern example
recipeList:
  - io.moderne.ai.research.FindCodeThatResembles:
      resembles: HTTP request with Content-Type application/json
      k: 5
```

Now that `com.yourorg.FindCodeThatResemblesExample` has been defined, activate it and take a dependency on org.openrewrite.recipe:rewrite-ai-search:0.19.1 in your build file:
<Tabs groupId="projectType">
<TabItem value="gradle" label="Gradle">

1. Add the following to your `build.gradle` file:

```groovy title="build.gradle"
plugins {
    id("org.openrewrite.rewrite") version("6.27.1")
}

rewrite {
    activeRecipe("com.yourorg.FindCodeThatResemblesExample")
    setExportDatatables(true)
}

repositories {
    mavenCentral()
}

dependencies {
    rewrite("org.openrewrite.recipe:rewrite-ai-search:0.19.1")
}
```
2. Run `gradle rewriteRun` to run the recipe.
</TabItem>
<TabItem value="maven" label="Maven">

1. Add the following to your `pom.xml` file:

```xml title="pom.xml"
<project>
  <build>
    <plugins>
      <plugin>
        <groupId>org.openrewrite.maven</groupId>
        <artifactId>rewrite-maven-plugin</artifactId>
        <version>5.45.0</version>
        <configuration>
          <exportDatatables>true</exportDatatables>
          <activeRecipes>
            <recipe>com.yourorg.FindCodeThatResemblesExample</recipe>
          </activeRecipes>
        </configuration>
        <dependencies>
          <dependency>
            <groupId>org.openrewrite.recipe</groupId>
            <artifactId>rewrite-ai-search</artifactId>
            <version>0.19.1</version>
          </dependency>
        </dependencies>
      </plugin>
    </plugins>
  </build>
</project>
```
2. Run `mvn rewrite:run` to run the recipe.
</TabItem>
<TabItem value="moderne-cli" label="Moderne CLI">

You will need to have configured the [Moderne CLI](https://docs.moderne.io/moderne-cli/cli-intro) on your machine before you can run the following command.

```shell title="shell"
mod run . --recipe FindCodeThatResemblesExample
```
</TabItem>
</Tabs>

## See how this recipe works across multiple open-source repositories

import RecipeCallout from '@site/src/components/ModerneLink';

<RecipeCallout link="https://app.moderne.io/recipes/io.moderne.ai.research.FindCodeThatResembles" />

The community edition of the Moderne platform enables you to easily run recipes across thousands of open-source repositories.

Please [contact Moderne](https://moderne.io/product) for more information about safely running the recipes on your own codebase in a private SaaS.
## Data Tables

### Code Search
**io.moderne.ai.table.CodeSearch**

_Searches for method invocations that resemble a natural language query._

| Column Name | Description |
| ----------- | ----------- |
| Source | Source |
| Method | Method invocation |
| Query | Natural language query |
| Result of first models | First two embeddings models result, where -1 means negative match, 0 means unsure, and 1 means positive match. |
| Called second model | True if the generative model was used. |
| Result of second model | Second generative model's result. |

### Top-K Method Matcher
**io.moderne.ai.table.TopKMethodMatcher**

_Result from the scanning recipe for top-k method patterns that match the query._

| Column Name | Description |
| ----------- | ----------- |
| Method Pattern | Method invocation pattern. |
| Method Signature | Method invocation signature. |
| Distance | The distance between the query and the method invocation. |
| Query | The natural language search query. |

### Embedding performance
**io.moderne.ai.table.EmbeddingPerformance**

_Latency characteristics of uses of embedding models._

| Column Name | Description |
| ----------- | ----------- |
| Source file | The source file that the method call occurred in. |
| Number of requests | The count of requests made to the model. |
| Histogram | The latency histogram of the requests made to the model (counts). The histogram is a non-cumulative fixed distribution of 100 buckets of 0.01 second each. |
| Max latency | The maximum embedding latency. |

### Generative model performance
**io.moderne.ai.table.GenerativeModelPerformance**

_Latency characteristics of uses of generative models._

| Column Name | Description |
| ----------- | ----------- |
| Source file | The source file that the method call occurred in. |
| Number of requests | The count of requests made to the model. |
| Histogram | The latency histogram of the requests made to the model (counts). The histogram is a non-cumulative fixed distribution of 100 buckets of 1 second each. |
| Max latency | The maximum embedding latency. |

### Suggested method patterns
**io.moderne.ai.table.SuggestedMethodPatterns**

_As the next step after the AI-based searching for method invocations, you may want to do rule-based method searching using the recommended method patterns._

| Column Name | Description |
| ----------- | ----------- |
| Method | Method invocation |
| Method Pattern | Method invocation pattern. |
| Query | The natural language search query. |

### Source files that had results
**org.openrewrite.table.SourcesFileResults**

_Source files that were modified by the recipe run._

| Column Name | Description |
| ----------- | ----------- |
| Source path before the run | The source path of the file before the run. `null` when a source file was created during the run. |
| Source path after the run | A recipe may modify the source path. This is the path after the run. `null` when a source file was deleted during the run. |
| Parent of the recipe that made changes | In a hierarchical recipe, the parent of the recipe that made a change. Empty if this is the root of a hierarchy or if the recipe is not hierarchical at all. |
| Recipe that made changes | The specific recipe that made a change. |
| Estimated time saving | An estimated effort that a developer to fix manually instead of using this recipe, in unit of seconds. |
| Cycle | The recipe cycle in which the change was made. |

### Source files that errored on a recipe
**org.openrewrite.table.SourcesFileErrors**

_The details of all errors produced by a recipe run._

| Column Name | Description |
| ----------- | ----------- |
| Source path | The file that failed to parse. |
| Recipe that made changes | The specific recipe that made a change. |
| Stack trace | The stack trace of the failure. |

### Recipe performance
**org.openrewrite.table.RecipeRunStats**

_Statistics used in analyzing the performance of recipes._

| Column Name | Description |
| ----------- | ----------- |
| The recipe | The recipe whose stats are being measured both individually and cumulatively. |
| Source file count | The number of source files the recipe ran over. |
| Source file changed count | The number of source files which were changed in the recipe run. Includes files created, deleted, and edited. |
| Cumulative scanning time | The total time spent across the scanning phase of this recipe. |
| 99th percentile scanning time | 99 out of 100 scans completed in this amount of time. |
| Max scanning time | The max time scanning any one source file. |
| Cumulative edit time | The total time spent across the editing phase of this recipe. |
| 99th percentile edit time | 99 out of 100 edits completed in this amount of time. |
| Max edit time | The max time editing any one source file. |

