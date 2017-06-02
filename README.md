Unilur
================
Eric Koncina

### Overview

**`unilur`** is a R package to help writing tutorials, practicals or examination papers with [rmarkdown](http://rmarkdown.rstudio.com/).

With `unilur` you can render the following outputs from a single rmarkdown file:

-   the exam or tutorial questions (answers remaining hidden) as a PDF or HTML file.
-   the exam or tutorial questions with sample answers as a PDF or HTML file.

In addition, you will be able to:

-   Create coloured boxes to highlight some markdown or *R* content.
-   Create examination papers with
    -   multiple choice questions
    -   a candidate identification form
    -   dotted lines placeholders to fill in answers
-   Create a new Rmarkdown file with solution chunks replaced by empty ones.

### Installation

The source code is hosted on [github](https://github.com/koncina/unilur). To install the *R* package, run the following code:

``` r
devtools::install_github("koncina/unilur")
```

The package contains example templates illustrating some of the possibilities.

### Writing tutorials

To create tutorials, adjust the `output` format in the document header as follows:

    output:
      unilur::tutorial_html: default
      unilur::tutorial_pdf: default
      unilur::tutorial_html_solution: default
      unilur::tutorial_pdf_solution: default
      unilur::answer_rmd: default

Use the Rstudio knit button and knit to the wanted output format. Keep in mind that the output file will end with a suffix (which can be adjusted in the YAML header):

-   **`_question`** for `unilur::tutorial_html` and `unilur::tutorial_pdf`
-   **`_solution`** for `unilur::tutorial_html_solution` and `unilur::tutorial_pdf_solution`
-   **`_answer`** for `unilur::answer_rmd`

#### Yaml header arguments

Adjust additional parameters of the `unilur` format in the YAML front-matter. The table below summarises the different settings (default values are **bold**):

| parameter        | value                   | description                              |
|------------------|-------------------------|:-----------------------------------------|
| question\_suffix | *text* (**\_question**) | suffix appended to the question pdf/html |
| solution\_suffix | *text* (**\_solution**) | suffix appended to the solution pdf/html |
| answer\_suffix   | *text* (**\_answer**)   | suffix appended to the answer Rmd        |
| credit           | `yes`/**`no`**          | Show a link to this homepage             |

#### Solutions

Answers or solutions are written within a [code chunk](http://rmarkdown.rstudio.com/authoring_rcodechunks.html) with the `solution = TRUE` option. The content of these chunks will be wrapped within a green box.

<img src="http://eric.koncina.eu/pics/r/unilur/solution.jpg" style="display: block; margin: auto;" />

#### Coloured boxes

To highlight some content and wrap it within a coloured box, use the `box = "<colour>"` chunk option. Use the colours defined by `colors()` or define your own (using the `rgb()` function or a hexadecimal colour code). You can adjust the box title using the `boxtitle = "<title>"` chunk option.

    ```{asis, box = "red", boxtitle = "My red box"}
    This is a **red** box and titled *My red box*
    ```

    ```{asis, box = "green"}
    This is a **green** box without title
    ```

    ```{asis, box = rgb(159, 129, 247, maxColorValue = 255), boxtitle = "Custom color"}
    It is also possible to use **custom colors** defined as hexadecimal color codes or using the `rgb()` function. 
    ```

<img src="http://eric.koncina.eu/pics/r/unilur/colorbox.jpg" style="display: block; margin: auto;" />

### Writing examination papers

The `unilur::examen_pdf` and `unilur::examen_pdf_solution` templates (relying on the latex [exam class](https://www.ctan.org/pkg/exam)) extend the possibilities of the tutorial template:

-   include an **identification box** to be filled in by the candidate
-   insert **placeholders with dotted lines** between questions to write in the answers
-   generate **multiple choice questions**

#### Yaml header arguments

<table>
<colgroup>
<col width="7%" />
<col width="57%" />
<col width="34%" />
</colgroup>
<thead>
<tr class="header">
<th>argument</th>
<th>value</th>
<th align="left">description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td><strong><code>yes</code></strong>/<code>no</code></td>
<td align="left">draws a candidate identification form</td>
</tr>
<tr class="even">
<td>mcq</td>
<td><code>oneparchoices</code>, <code>oneparchoicesalt</code>, <code>oneparcheckboxesalt</code>, <code>oneparcheckboxes</code></td>
<td align="left">theme for the multiple choice questions</td>
</tr>
</tbody>
</table>

#### Questions

Questions are defined as fifth-level 5 headings (`##### question`). It is possible to leave some space with lines in the main pdf file allowing the students to write down their answers. Just set the `response.space = <s>` chunk argument (in inches) together with `solution = TRUE`.

    ---
    title: "Examen demo"
    author: "John Doe"
    date: "19 April, 2016"
    output:
      unilur::examen_pdf:
        id: yes
      unilur::examen_pdf_solution:
        id: yes
    ---

    ##### Write your answer below

    ```{asis, solution = TRUE, response.space = 1}
    This is the hidden answer generating lines in the main pdf file.
    ```

    ##### Second question

    ```{asis, solution = TRUE, response.space = 0.5}
    This would be the expected answer to the question
    ```

<img src="http://eric.koncina.eu/pics/r/unilur/exam_questions.jpg" style="display: block; margin: auto;" /><img src="http://eric.koncina.eu/pics/r/unilur/exam_solution.jpg" style="display: block; margin: auto;" />

#### Multiple choice questions

Lists can be rendered as multiple choice questions. Write the list within an `asis` chunk and set the option `mcq = TRUE` like in the example below:

    ```{asis, mcq = TRUE}
    - Item 1
    - Item 2
    - Item 3
    - Item 4
    - Item 5
    ```

The output theme of MCQ can be adjusted in the YAML front-matter as shown below:

    output:
      unilur::examen_pdf:
        mcq: <mcq theme>

The snapshots below show how the different `<mcq theme>` values look like:

|       mcq theme       |                                 result                                |
|:---------------------:|:---------------------------------------------------------------------:|
|    `oneparchoices`    |    ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparchoices.jpg)    |
|   `oneparchoicesalt`  |   ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparchoicesalt.jpg)  |
|   `oneparcheckboxes`  |   ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparcheckboxes.jpg)  |
| `oneparcheckboxesalt` | ![](http://eric.koncina.eu/pics/r/unilur/mcq_oneparcheckboxesalt.jpg) |

For the `oneparchoicesalt` and `oneparcheckboxesalt` options, it is possible to specify the number of items per line with the `mcq.n` chunk option (default is 3).

### Generate a Rmarkdown file without solution chunks

Use the `unilur::answer_rmd` output format to generate a Rmarkdown document containing all the instructions but without the solution chunks which are replaced by empty ones. The Yaml header is also changed and you can adjust the title, author or output:

    output:
      unilur::answer_rmd:
        yaml:
          title: "My answers"
          author: "My name"
          output: html_document
