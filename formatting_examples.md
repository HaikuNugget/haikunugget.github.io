---
layout: page
title: Formatting Examples
permalink: /formatting-examples
tagline: Cayman Yeah.
ref: formatting-examples-2
order: 3
---
# An Overview of Formatting

<!-- begin toc -->
# Table of Contents
Pro Tip: Table of contents string match a header line. (note: missing period on #5)
1. [Example](#example)
2. [Example2](#example2)
3. [Third Example](#third-example)
4. [Fourth Example](#fourth-examplehttpwwwfourthexamplecom)
5. [Some stuff in Index](#some-stuff-in-index)


## Example
## Example2
### Third Example
## [Fourth Example](http://www.fourthexample.com)
<!-- end toc -->

## [More detailed Markdown guide reference can be found here (offsite link)](https://www.markdownguide.org/basic-syntax/)

#### Some stuff in Index.

---

To create a horizontal rule, use three or more asterisks (***), dashes (---), or underscores (___) on a line by themselves.

***

## Quoting and code formatting examples

`One qoute: code line To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults`

```Three quote: To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults```

<pre>pre-tags: This allows scroll left-right and one line copy. To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults</pre>

``` bash 
$ kubectl --something
# ls -l
# oci get
# aws ec2 describe instances
```

```bash
# echo 'The type of code formatted can be specified after the 3 backticks ```'
The type of code formatted can be specified after the 3 backticks ```
# aws ec2 describe instances
...text removed...
# ls -l
```

```python

def execute_arbitrary_code () {
    random_output = "Random code"
    print($random_output)

    return $random_output


if __name__ == '__main__':
    execute_arbitrary_code()


```

### The `kubectl` example below using the "code" block type.
### It doesn't work right, stick with 3 backticks instead.
```markdown
<code>
$ kubectl --something
# ls -l
</code>
```

```markdown
# oci get
# aws ec2 describe instances
```

## An H2
An example of code quoting mid-sentence `code quoting` is supplied here.

### This H3 header nicely introduces a table
Table formatting as follows:

|  The | quick | brown | fox | jumped |
|---|---|---|---|---|
|  over |   |   |   |   |
|  the |   |   |   |   |
|  lazy |   |   |   |   |
|  yellow |   |   |   |   |
|  dog |   |   |   |   |

#### List
This list is followed by a blockquote example
* blockquote examples 
  * two
    * three
  * two
* one

#### Blockquote
> Yet another example of the quick brown fox jumping over the lazy yellow dog. 
The quick brown fox jumps over the lazy yellow dog.
The quick brown fox jumps over the lazy yellow dog, again.
The quick brown fox jumps over the lazy yellow dog, for a third time.

> blockquote these 3 are supposed to be one `blockquote`, they're not read the docs
> blockquote
> blockquote
> blockquote
