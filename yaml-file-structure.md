# YAML imports

If your subscription plan includes YAML imports, you can use the structure described below to import Choice, Code Gaps or Essay tasks.

## Formatting tips
Below are a few hints on how to create a perfect YAML import file.

1. Divide tasks with one empty paragraph in between.
2. Markdown can be used for formatting, including the syntax for code blocks:

    a) For the inline code snippets use backticks around the snippet, like in markdown.

    ```
        `echo "Hello World!";`
    ```

    b) For the block of code use 3 backticks at the beginning and 3 backticks at the end of code
    block.
    
    ```
        ```
        echo "Hello";
        echo "World!";
        ```
    ```

3. Pay attention to whitespaces and quoting of special characters - they can break your task and it will not pass the validation. You can use [YAML linters](https://yamlchecker.com/) to streamline your YAML files creation.

    There are multiple ways to achieve a line continuation in YAML:

    - **Folded style** - removes the newlines within the string (but adds one at the end).
    
    ```yaml
    title: >-
      JavaScript DEMO question
    ```

    - **Literal style** - turns newlines within the string into literal newlines

    ```yaml
    title: |-
      JavaScript DEMO question
    ```


## Choice questions

### Correct and wrong answers
You can create two types of questions: multiple and single choice questions.

It is pretty obvious that in single choice questions there could be a lot of possible answers but one correct, but in multiple choice questions there can be any combination of correct vs wrong answers. 
For example: 2 correct and 5 wrong; 4 correct and 1 wrong and etc.

### YAML format

A YAML file should consist of a list of questions. 
Every question is a hash, which must contain the following keys.

- `uuid` - id of the task. With this id you will be able to update tasks. You can use existing ones or generate unique values using [generators](https://www.uuidgenerator.net/).
- `difficulty` - the difficulty level, one of `EASY`, `MEDIUM`, `HARD`.
- `duration` - time duration in minutes or in the ISO 8601 duration format.
- `points` - default number of points for this task.
- `tags` - task tags, ie. what kind of knowledge this questions tests.
- `question` - the question which will be presented to the candidate.
- `type` - type of a task: `MULTI_CHOICE`.
- `action` - put this key to decide whether you want your question be automatically published on addition. Possible values are: `PUBLISH` or `CREATE_DRAFT` _(default)_.
- `mode` - add this one with SINGLE key word if you need a single choice question.
-  `choices` - list of hashes with choices. The key in these hashes can be either `correct`, for correct answers, or `wrong`, for wrong ones.

**Single choice question example:**
```yaml
- uuid: 8d002491-1e45-4f57-9f2b-149393cbd47d
  title: JavaScript | Demo question
  difficulty: EASY
  duration: 2
  points: 5
  tags: [JavaScript, SecondTag]
  question: |-
    Select the correct statement about JavaScript.
  type: MULTI_CHOICE
  action: PUBLISH
  mode: SINGLE
  choices:
    - correct: |-
        JavaScript is a dynamic programming language, which can be used to write client-side scripts for web browsers.
    - wrong: |- 
        JavaScript applications are compiled to bytecode that can run on a Java Virtual Machine.
    - wrong: |-
        JavaScript was originally developed by James Gosling at Sun  Microsystems
```

**Multiple choice question example:**
```yaml
- uuid: 8d002491-1e45-4f57-9f2b-149393cbd47d
  title: JavaScript | Demo question
  difficulty: EASY
  duration: 'PT2M'
  points: 5
  tags: [JavaScript, SecondTag]
  question: |-
    Select the correct statement about JavaScript.
  type: MULTI_CHOICE
  action: CREATE_DRAFT
  choices:
    - correct: |-
        JavaScript is a dynamic programming language, which can be used to write client-side scripts for web browsers.
    - wrong: |-
        JavaScript applications are compiled to bytecode that can run on a Java Virtual Machine.
    - wrong: |-
        JavaScript was originally developed by James Gosling at Sun Microsystems
    - correct: |-
        I am also correct.
```

## Code Gaps questions
Code gaps is a multipurpose task. You can provide a snippet of code with gaps in it where the candidate must fill in the blanks to make it complete.

### YAML format

A YAML file should consist of a list of questions. 
Every question is a hash, which must contain the following keys.

- `uuid` - id of the task. With this id you will be able to update tasks. You can use existing ones or generate unique values using [generators](https://www.uuidgenerator.net/).
- `title` - title of a task.
- `difficulty` - the difficulty level, one of `EASY`, `MEDIUM`, `HARD`.
- `duration` - time duration in minutes or in the ISO 8601 duration format.
- `points` - default number of points for this task.
- `tags` - task tags, ie. what kind of knowledge this questions tests.
- `question` - the question which will be presented to the candidate.
- `type` - type of a task: `CODE_GAPS`.
- `action` - put this key to decide whether you want your question be automatically published on addition. Possible values are: `PUBLISH` or `CREATE_DRAFT` _(default)_.
- `mode` - syntax highlight mode, e.g.: `PLAIN` _(regular text)_, `Dockerfile`, `PHP`, `Java`
- `content` - the task code.

### Content field format:
Code gaps should be covered with brackets `{{{gap}}}`. 

If more than 2 choices can be correct, put `||` in front of each correct answer. Each task can have more than one gap.

You can also combine rules for code gaps. For example:

`{{{|CW|"hello"|CW|'hello'}}}` 

This means that the correct answer can have `'` or `"` quotes and ignore case + ignore whitespace options enabled.

- `|C|` - stands for **ignore case** in the answer.
- `|R|` - stands for **regexp** in the answer.
- `|W|` - stands for **ignore whitespace** in the answer. If the answer consists only from 1 word no `|W|` is needed.

If your task uses `{` and `}` as part of the code, you should use the following format:

```
{{{ {this is all code gap} }}}
```

**Single answer example:**
```yaml
- uuid: 4da801c5-b132-43d1-a211-8e5efb43cffa
  title: DevOps | Running docker containers - cleanup
  difficulty: EASY
  duration: 3
  points: 2
  tags: [DevOps, Docker]
  question: |-
    Ensure that the container will be removed when it exits
  type: CODE_GAPS
  mode: SHELL
  action: PUBLISH
  content: |-
    $ docker run {{{--rm}}} hello-world
```

**Multiple answers example:**
```yaml
- uuid: 54454be6-38f8-4707-871e-31ccedef79f1
  title: JavaScript | Some unique task name
  difficulty: EASY
  duration: 'PT10M'
  points: 10
  tags: [JavaScript]
  skills: [Software Development, JavaScript]
  question: |-
    Fill in the JavaScript code gap to make the code do this and that.
  type: CODE_GAPS
  mode: JAVASCRIPT
  action: PUBLISH
  content: |-
    here goes the code and here goes the {{{|C|gap|C|gaps}}}
```

## Essay questions
Essay is a open-ended and manually evaluated task where the candidate can write a short answer.

### YAML format

A YAML file should consist of a list of questions. 
Every question is a hash, which must contain the following keys.

- `uuid` - id of the task. With this id you will be able to update tasks. You can use existing ones or generate unique values using [generators](https://www.uuidgenerator.net/).
- `title` - title of a task.
- `difficulty` - the difficulty level, one of `EASY`, `MEDIUM`, `HARD`.
- `duration` - time duration in minutes or in the ISO 8601 duration format.
- `points` - default number of points for this task.
- `tags` - task tags, ie. what kind of knowledge this questions tests.
- `question` - the question which will be presented to the candidate.
- `type` - type of a task: `ESSAY`.
- `action` - put this key to decide whether you want your question be automatically published on addition. Possible values are: `PUBLISH` or `CREATE_DRAFT` _(default)_.

**Example:**
```yaml
- uuid: 4da801c5-b132-43d1-a211-8e5efb43cffa
  title: Soft Skills | Some unique task name
  difficulty: MEDIUM
  duration: 30
  points: 15
  tags: ['Soft Skills']
  question: |-
    Describe a situation when you had to work in a team.
  type: ESSAY
  action: PUBLISH
```